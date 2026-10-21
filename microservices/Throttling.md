

public class ThrottlingDemo {
    // Minimum gap required between two requests (1000ms = 1 second)
    private static final long MIN_INTERVAL = 1000; 
    private static long lastRequestTime = 0;

    // Synchronized method to pace incoming requests
    public static synchronized void processRequest(int requestId) {
        long currentTime = System.currentTimeMillis();
        long timePassed = currentTime - lastRequestTime;

        // If the request came too quickly, delay it
        if (timePassed < MIN_INTERVAL) {
            long delay = MIN_INTERVAL - timePassed;
            try {
                Thread.sleep(delay);
            } catch (InterruptedException e) {
                System.out.println(e.getMessage());
            }
        }

        // Print the time when the request is actually processed
        System.out.println("Request " + requestId + " processed at: " + (System.currentTimeMillis() / 1000) % 60 + "s");
        lastRequestTime = System.currentTimeMillis();
    }

    public static void main(String[] args) {
        System.out.println("Starting Throttling POC (Max 1 Request/Second)...");
        
        // Simulating 5 rapid incoming requests
        for (int i = 1; i <= 5; i++) {
            processRequest(i);
        }
    }
}
