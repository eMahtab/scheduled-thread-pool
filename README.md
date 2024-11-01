# Scheduled Thread Pool

```java
import java.util.concurrent.Executors;
import java.util.concurrent.ScheduledExecutorService;
import java.util.concurrent.TimeUnit;

class SystemStatusChecker {

    private final ScheduledExecutorService scheduler = Executors.newScheduledThreadPool(1);
    private static final int INITIAL_DELAY = 5;
    private static final int CHECK_INTERVAL = 10;
    private static final int SHUTDOWN_TIMEOUT = 5;

    public void startSystemCheck() {
        scheduler.scheduleAtFixedRate(this::checkSystemStatus, INITIAL_DELAY, CHECK_INTERVAL, TimeUnit.SECONDS);
        System.out.println("System status checker started.");
    }

    private void checkSystemStatus() {
        System.out.println("Checking system status at " + System.currentTimeMillis());

        // Simulate system health check logic
        double healthCheckOutcome = Math.random();
        if (healthCheckOutcome < 0.5) {
            System.out.println("ALERT: System health warning!");
        } else {
            System.out.println("System status: OK");
        }
    }

    public void stopSystemCheck() {
        System.out.println("Stopping system status checker...");
        scheduler.shutdown();
        try {
            if (!scheduler.awaitTermination(SHUTDOWN_TIMEOUT, TimeUnit.SECONDS)) {
                System.out.println("Forcing shutdown due to delay.");
                scheduler.shutdownNow();
            } else {
                System.out.println("System status checker stopped successfully.");
            }
        } catch (InterruptedException e) {
            System.err.println("Scheduler interrupted during shutdown.");
            scheduler.shutdownNow();
            Thread.currentThread().interrupt();
        }
    }
}

public class Main {
    public static void main(String[] args) {
        SystemStatusChecker checker = new SystemStatusChecker();
        checker.startSystemCheck();

        // Simulate running the checker for 1 minute
        try {
            Thread.sleep(60000);
        } catch (InterruptedException e) {
            System.err.println("Main thread interrupted.");
            Thread.currentThread().interrupt();
        }

        checker.stopSystemCheck();
    }
}

```
