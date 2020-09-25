# Delay-in-java
How to give delay without Thread.sleep() in Java

import java.util.concurrent.Callable;
import java.util.concurrent.ExecutionException;
import java.util.concurrent.Executors;
import java.util.concurrent.ScheduledExecutorService;
import java.util.concurrent.ScheduledFuture;
import java.util.concurrent.TimeUnit;


public class SleepAlternate {

    public static void main(String[] args) throws ExecutionException, InterruptedException {
        ScheduledExecutorService executorService = Executors.newSingleThreadScheduledExecutor();
        Callable<String> supplier = () -> execute("from supplier");
        ScheduledFuture<?> scheduledFuture = executorService.schedule(supplier, 10, TimeUnit.SECONDS);
        System.out.println("Printed before supplier schedule at: "+ System.currentTimeMillis());
        System.out.println(scheduledFuture.get());
        System.out.println("Printed after supplier schedule");

        scheduledFuture = executorService.schedule(runnable(), 10, TimeUnit.SECONDS);
        System.out.println("Printed before runnable schedule: "+ System.currentTimeMillis());
        scheduledFuture.get();
        System.out.println("Printed after runnable schedule");
        executorService.shutdown();
    }

    public static String execute(String testMessage) {
        return "Printed at "+ System.currentTimeMillis() + ": " + testMessage;
    }

    public static Runnable runnable() {
        return new Runnable() {
            int attempt = 0;

            public void run() {
                System.out.println("ran at "+ System.currentTimeMillis() + " run no: " + ++attempt);
            }
        };
    }
    
    Output:
Printed before supplier schedule at: 1601056947630
Printed at 1601056957631: from supplier
Printed after supplier schedule
Printed before runnable schedule: 1601056957632
ran at 1601056967633 run no: 1
Printed after runnable schedule



}
