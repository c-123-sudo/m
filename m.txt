1 a).
public class bigO {
    public static void main(String[] args) {
        int c = 8;
        int n0 = -1;

        System.out.println(" n  |  f(n) = 7n + 5  |  c * g(n) = 8n");
        System.out.println("---------------------------------------");

        for (int n = 1; n <= 10; n++) {
            int fn = 7 * n + 5;
            int gn = c * n;

            System.out.printf("%2d  |    %3d          |    %3d\n", n, fn, gn);

            if (fn <= gn && n0 == -1) {
                n0 = n;
            }
        }

        if (n0 != -1) {
            System.out.println("\nThe smallest n0 where f(n) <= c * g(n) is: " + n0);
        } else {
            System.out.println("\nNo such n0 found in the given range.");
        }
    }
}

1 b). 
public class GaleShapley {

    private int N;
    private String[][] menPref;
    private String[][] womenPref;
    private String[] men;
    private String[] women;
    private String[] womenPartner;
    private boolean[] menEngaged;


    public GaleShapley(String[] men, String[] women, String[][] menPref, String[][] womenPref) {
        this.N = men.length;
        this.men = men;
        this.women = women;
        this.menPref = menPref;
        this.womenPref = womenPref;
        this.womenPartner = new String[N];
        this.menEngaged = new boolean[N];
        calculateMatches();
    }


    private void calculateMatches() {
        int engagedCount = 0;

        while (engagedCount < N) {
            int freeMan;
            for (freeMan = 0; freeMan < N; freeMan++) {
                if (!menEngaged[freeMan]) break;
            }

            for (int i = 0; i < N && !menEngaged[freeMan]; i++) {
                String woman = menPref[freeMan][i];
                int womanIndex = getIndex(woman, women);

                if (womenPartner[womanIndex] == null) {

                    womenPartner[womanIndex] = men[freeMan];
                    menEngaged[freeMan] = true;
                    engagedCount++;
                } else {

                    String currentPartner = womenPartner[womanIndex];
                    if (prefersNewPartner(womanIndex, men[freeMan], currentPartner)) {

                        int currentPartnerIndex = getIndex(currentPartner, men);
                        menEngaged[currentPartnerIndex] = false;
                        womenPartner[womanIndex] = men[freeMan];
                        menEngaged[freeMan] = true;
                    }
                }
            }
        }
    }


    private boolean prefersNewPartner(int womanIndex, String newMan, String currentMan) {
        for (String man : womenPref[womanIndex]) {
            if (man.equals(newMan)) return true;
            if (man.equals(currentMan)) return false;
        }
        return false;
    }


    private int getIndex(String name, String[] list) {
        for (int i = 0; i < list.length; i++) {
            if (list[i].equals(name)) return i;
        }
        return -1;
    }


    public void printMatches() {
        System.out.println("Stable Matches:");
        for (int i = 0; i < N; i++) {
            System.out.println(womenPartner[i] + " is matched with " + women[i]);
        }
    }


    public static void main(String[] args) {
        String[] men = {"A", "B", "C"};
        String[] women = {"V", "W", "X"};

        String[][] menPref = {
            {"V", "W", "X"},
            {"W", "V", "X"},
            {"V", "W", "X"}
        };

        String[][] womenPref = {
            {"A", "B", "C"},
            {"B", "C", "A"},
            {"C", "A", "B"}
        };

        GaleShapley gs = new GaleShapley(men, women, menPref, womenPref);
        gs.printMatches();
    }
}

2a).
public class omega {
    public static void main(String[] args) {
        int c = 1;       // constant c
        int n0 = -1;     // to store smallest n0 where f(n) >= c*g(n)

        System.out.println(" n  |  f(n) = 3n^2 + 4n + 3  |  c * g(n) = " + c + " * n^2");
        System.out.println("-----------------------------------------------");

        // Check for n from 10 to 30 as per your request
        for (int n = 10; n <= 30; n++) {
            int fn = 3 * n * n + 4 * n + 3;
            int cg = c * n * n;

            System.out.printf("%2d  |        %5d           |       %3d\n", n, fn, cg);

            // Find smallest n0 where f(n) >= c*g(n)
            if (fn >= cg && n0 == -1) {
                n0 = n;
            }
        }

        if (n0 != -1) {
            System.out.println("\nThe smallest n0 where f(n) >= c * g(n) is: " + n0);
        } else {
            System.out.println("\nNo such n0 found in the given range.");
        }
    }
}

b).
import java.util.*;

class djk {
    static class Edge {
        int target;
        int weight;
        Edge(int target, int weight) {
            this.target = target;
            this.weight = weight;
        }
    }

    static Map<Integer, List<Edge>> buildGraph() {
        Map<Integer, List<Edge>> graph = new HashMap<>();
        for (int i = 1; i <= 5; i++) {
            graph.put(i, new ArrayList<>());
        }

        // Add edges as per the image
        graph.get(1).add(new Edge(2, 20));
        graph.get(1).add(new Edge(3, 30));
        graph.get(1).add(new Edge(4, 10));

        graph.get(2).add(new Edge(3, 40));

        graph.get(3).add(new Edge(5, 20));

        graph.get(4).add(new Edge(3, 5));
        graph.get(4).add(new Edge(5, 20));

        return graph;
    }

    static void dijkstra(Map<Integer, List<Edge>> graph, int source) {
        Map<Integer, Integer> distance = new HashMap<>();
        for (int node : graph.keySet()) {
            distance.put(node, Integer.MAX_VALUE);
        }
        distance.put(source, 0);

        PriorityQueue<int[]> pq = new PriorityQueue<>(Comparator.comparingInt(a -> a[1]));
        pq.offer(new int[]{source, 0});
        Set<Integer> visited = new HashSet<>();

        while (!pq.isEmpty()) {
            int[] current = pq.poll();
            int node = current[0];
            int dist = current[1];

            if (visited.contains(node)) continue;
            visited.add(node);

            for (Edge edge : graph.get(node)) {
                int neighbor = edge.target;
                int newDist = dist + edge.weight;
                if (newDist < distance.get(neighbor)) {
                    distance.put(neighbor, newDist);
                    pq.offer(new int[]{neighbor, newDist});
                }
            }
        }

        System.out.println("Reachable Nodes from Node " + source + ":");
        for (int node : distance.keySet()) {
            if (distance.get(node) != Integer.MAX_VALUE) {
                System.out.println("Node " + node + " at distance " + distance.get(node));
            }
        }
    }

    public static void main(String[] args) {
        Map<Integer, List<Edge>> graph = buildGraph();
        dijkstra(graph, 1);  // Source node = 1
    }
}


3 a).
public class theta {
    public static void main(String[] args) {
        int c1 = 1;
        int c2 = 8;
        int n0 = -1;

        System.out.println(" n  |  f(n) = 7n*n + 7n + 5  |  c1 * g(n) = 8n*n  |  c2 * g(n) = 8n*n");
        System.out.println("--------------------------------------------------------------------");

        for (int n = 1; n <= 10; n++) {
            int fn = 7 * n * n + 7 * n  + 5;
            int c1gn = c1 * n * n;
            int c2gn = c2 * n * n;

            System.out.printf("%2d  |          %3d          |       %3d          |    %3d\n", n, fn, c1gn, c2gn);

            if (c1gn <= fn && fn <= c2gn && n0 == -1) {
                n0 = n;
            }
        }

        if (n0 != -1) {
            System.out.println("\nThe smallest n0 where c1 * gn <= f(n) <= c2 * g(n) is: " + n0);
        } else {
            System.out.println("\nNo such n0 found in the given range.");
        }
    }
}
b). 
import java.util.Random;

public class MergeSortTimer {

    // Merge Sort
    public static void mergeSort(int[] arr, int left, int right) {
        if (left < right) {
            int mid = (left + right) / 2;
            mergeSort(arr, left, mid);
            mergeSort(arr, mid + 1, right);
            merge(arr, left, mid, right);
        }
    }

    public static void merge(int[] arr, int left, int mid, int right) {
        int[] leftArr = new int[mid - left + 1];
        int[] rightArr = new int[right - mid];

        for (int i = 0; i < leftArr.length; i++) leftArr[i] = arr[left + i];
        for (int j = 0; j < rightArr.length; j++) rightArr[j] = arr[mid + 1 + j];

        int i = 0, j = 0, k = left;
        while (i < leftArr.length && j < rightArr.length) {
            arr[k++] = (leftArr[i] <= rightArr[j]) ? leftArr[i++] : rightArr[j++];
        }

        while (i < leftArr.length) arr[k++] = leftArr[i++];
        while (j < rightArr.length) arr[k++] = rightArr[j++];
    }

    // Main method to test and time merge sort
    public static void main(String[] args) {
        int[] sizes = {100, 200, 300, 500, 1000, 2000, 3000, 5000, 6000, 8000, 10000, 50000};

        System.out.println("n\tTime (ms)");
        for (int n : sizes) {
            int[] data = generateRandomArray(n);

            long start = System.nanoTime();
            mergeSort(data, 0, data.length - 1);
            long end = System.nanoTime();

            long timeTaken = (end - start)/1000000; // convert to milliseconds
            System.out.println(n + "\t" + timeTaken);
        }
    }

    // Utility method to generate random integers
    public static int[] generateRandomArray(int size) {
        Random rand = new Random();
        int[] arr = new int[size];
        for (int i = 0; i < size; i++) arr[i] = rand.nextInt(size * 10);
        return arr;
    }
}

4a).
b).
import java.util.Arrays;

public class inversioncounter {

    // Function to count inversions using merge sort
    public static int countInversions(int[] arr) {
        if (arr == null || arr.length < 2) return 0;
        return mergeSortAndCount(arr, 0, arr.length - 1);
    }

    private static int mergeSortAndCount(int[] arr, int left, int right) {
        int count = 0;
        if (left < right) {
            int mid = (left + right) / 2;
            count += mergeSortAndCount(arr, left, mid);
            count += mergeSortAndCount(arr, mid + 1, right);
            count += mergeAndCount(arr, left, mid, right);
        }
        return count;
    }

    private static int mergeAndCount(int[] arr, int left, int mid, int right) {
        int[] leftArr = Arrays.copyOfRange(arr, left, mid + 1);
        int[] rightArr = Arrays.copyOfRange(arr, mid + 1, right + 1);

        int i = 0, j = 0, k = left, swaps = 0;

        while (i < leftArr.length && j < rightArr.length) {
            if (leftArr[i] <= rightArr[j]) {
                arr[k++] = leftArr[i++];
            } else {
                arr[k++] = rightArr[j++];
                swaps += (mid + 1) - (left + i); // Count inversions
            }
        }

        while (i < leftArr.length) arr[k++] = leftArr[i++];
        while (j < rightArr.length) arr[k++] = rightArr[j++];

        return swaps;
    }

    public static void main(String[] args) {
        // Playlists of 3 users (each playlist is a permutation of songs 1 to 8)
        int[] user1 = {3, 1, 2, 4, 5, 6, 7, 8};
        int[] user2 = {1, 2, 3, 4, 5, 6, 7, 8};
        int[] user3 = {2, 3, 1, 5, 4, 6, 8, 7};

        // Count inversions for each user playlist
        int inv1 = countInversions(Arrays.copyOf(user1, user1.length));
        int inv2 = countInversions(Arrays.copyOf(user2, user2.length));
        int inv3 = countInversions(Arrays.copyOf(user3, user3.length));

        System.out.println("User 1 Inversions: " + inv1);
        System.out.println("User 2 Inversions: " + inv2);
        System.out.println("User 3 Inversions: " + inv3);

        int[] inversions = {inv1, inv2, inv3};

        // Recommend each user to follow the user with the closest inversion count
        for (int i = 0; i < inversions.length; i++) {
            int minDiff = Integer.MAX_VALUE;
            int recommendedUser = -1;
            for (int j = 0; j < inversions.length; j++) {
                if (i != j) {
                    int diff = Math.abs(inversions[i] - inversions[j]);
                    if (diff < minDiff) {
                        minDiff = diff;
                        recommendedUser = j + 1;
                    }
                }
            }
            System.out.println("Recommend User " + (i + 1) + " to follow User " + recommendedUser);
        }
    }
}

5a).
public class omega {
    public static void main(String[] args) {
        int c = 1;       // constant c
        int n0 = -1;     // to store smallest n0 where f(n) >= c*g(n)

        System.out.println(" n  |  f(n) = 2n + 3n + 5  |  c * g(n) = " + c + " * n");
        System.out.println("-----------------------------------------------");

        // Check for n from 10 to 30 as per your request
        for (int n = 10; n <= 30; n++) {
            int fn = 2 * n + 3 * n + 5;
            int cg = c * n;

            System.out.printf("%2d  |        %5d           |       %3d\n", n, fn, cg);

            // Find smallest n0 where f(n) >= c*g(n)
            if (fn >= cg && n0 == -1) {
                n0 = n;
            }
        }

        if (n0 != -1) {
            System.out.println("\nThe smallest n0 where f(n) >= c * g(n) is: " + n0);
        } else {
            System.out.println("\nNo such n0 found in the given range.");
        }
    }
}

b).
import java.util.*;

public class QuickSortExample {

    static void quickSort(int[] arr, int low, int high) {
        if (low < high) {
            int pIndex = partition(arr, low, high);
            quickSort(arr, low, pIndex - 1);
            quickSort(arr, pIndex + 1, high);
        }
    }

    static int partition(int[] arr, int low, int high) {
        int pivot = arr[high]; // last element as pivot
        int i = low - 1;

        for (int j = low; j < high; j++) {
            if (arr[j] <= pivot) {
                i++;
                // swap arr[i] and arr[j]
                int temp = arr[i]; arr[i] = arr[j]; arr[j] = temp;
            }
        }

        // Swap pivot to correct position
        int temp = arr[i + 1]; arr[i + 1] = arr[high]; arr[high] = temp;
        return i + 1;
    }

    public static void main(String[] args) {
        int[] data = {29, 10, 14, 37, 14, 12, 7, 23, 25, 16};

        System.out.println("Original Array: " + Arrays.toString(data));
        quickSort(data, 0, data.length - 1);
        System.out.println("Sorted Array:   " + Arrays.toString(data));
    }
}


6a).


b).
import java.util.*;

class KruskalsMST {
    static class Edge implements Comparable<Edge> {
        int src, dest, weight;

        Edge(int s, int d, int w) {
            src = s;
            dest = d;
            weight = w;
        }

        public int compareTo(Edge other) {
            return this.weight - other.weight;
        }
    }

    static class Subset {
        int parent, rank;
    }

    static int find(Subset[] subsets, int i) {
        if (subsets[i].parent != i)
            subsets[i].parent = find(subsets, subsets[i].parent);
        return subsets[i].parent;
    }

    static void union(Subset[] subsets, int x, int y) {
        int xroot = find(subsets, x);
        int yroot = find(subsets, y);

        if (subsets[xroot].rank < subsets[yroot].rank)
            subsets[xroot].parent = yroot;
        else if (subsets[xroot].rank > subsets[yroot].rank)
            subsets[yroot].parent = xroot;
        else {
            subsets[yroot].parent = xroot;
            subsets[xroot].rank++;
        }
    }

    static void kruskalMST(int V, List<Edge> edges) {
        Collections.sort(edges);

        Subset[] subsets = new Subset[V];
        for (int v = 0; v < V; ++v) {
            subsets[v] = new Subset();
            subsets[v].parent = v;
            subsets[v].rank = 0;
        }

        List<Edge> result = new ArrayList<>();
        int totalCost = 0;

        for (Edge edge : edges) {
            int x = find(subsets, edge.src);
            int y = find(subsets, edge.dest);

            if (x != y) {
                result.add(edge);
                totalCost += edge.weight;
                union(subsets, x, y);
            }

            if (result.size() == V - 1)
                break;
        }

        System.out.println("Edges in MST:");
        for (Edge e : result)
            System.out.println(e.src + " - " + e.dest + " : " + e.weight);
        System.out.println("Total Minimum Cost: " + totalCost);
    }

    public static void main(String[] args) {
            int V = 5; // number of cities/nodes
            List<Edge> edges = new ArrayList<>();

        // (u, v, cost)
        edges.add(new Edge(0, 1, 10));
        edges.add(new Edge(0, 4, 100));
        edges.add(new Edge(1, 2, 50));
        edges.add(new Edge(2, 4, 10));
        edges.add(new Edge(2, 3, 20));
        edges.add(new Edge(3, 4, 60));

        kruskalMST(V, edges);
    }
}
 

