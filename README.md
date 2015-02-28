# Quick-Sort-With-Tester-Method
import edu.princeton.cs.introcs.StdOut;

public class Quick {
	static File6 f5 = new File6();

	// This class should not be instantiated.
	private Quick() {
	}

	/**
	 * Rearranges the array in ascending order, using the natural order.
	 * 
	 * @param a
	 *            the array to be sorted
	 */
	public static void sort(Comparable[] a) {
		sort(a, 0, a.length - 1);
		assert isSorted(a);
	}

	// quicksort the subarray from a[lo] to a[hi]
	private static void sort(Comparable[] a, int lo, int hi) {
		if (hi <= lo)
			return;
		int j = partition(a, lo, hi);
		sort(a, lo, j - 1);
		sort(a, j + 1, hi);
		assert isSorted(a, lo, hi);
	}

	// partition the subarray a[lo..hi] so that a[lo..j-1] <= a[j] <= a[j+1..hi]
	// and return the index j.
	private static int partition(Comparable[] a, int lo, int hi) {
		int i = lo;
		int j = hi + 1;
		Comparable v = a[lo];
		while (true) {

			// find item on lo to swap
			while (less(a[++i], v))
				if (i == hi)
					break;

			// find item on hi to swap
			while (less(v, a[--j]))
				if (j == lo)
					break; // redundant since a[lo] acts as sentinel

			// check if pointers cross
			if (i >= j)
				break;

			exch(a, i, j);
		}

		// put partitioning item v at a[j]
		exch(a, lo, j);

		// now, a[lo .. j-1] <= a[j] <= a[j+1 .. hi]
		return j;
	}

	/**
	 * Rearranges the array so that a[k] contains the kth smallest key; a[0]
	 * through a[k-1] are less than (or equal to) a[k]; and a[k+1] through
	 * a[N-1] are greater than (or equal to) a[k].
	 * 
	 * @param a
	 *            the array
	 * @param k
	 *            find the kth smallest
	 */
	public static Comparable select(Comparable[] a, int k) {
		if (k < 0 || k >= a.length) {
			throw new IndexOutOfBoundsException(
					"Selected element out of bounds");
		}
		StdRandom.shuffle((Integer[]) a);
		int lo = 0, hi = a.length - 1;
		while (hi > lo) {
			int i = partition(a, lo, hi);
			if (i > k)
				hi = i - 1;
			else if (i < k)
				lo = i + 1;
			else
				return a[i];
		}
		return a[lo];
	}

	/***********************************************************************
	 * Helper sorting functions
	 ***********************************************************************/

	// is v < w ?
	private static boolean less(Comparable v, Comparable w) {
		return (v.compareTo(w) < 0);
	}

	// exchange a[i] and a[j]
	private static void exch(Object[] a, int i, int j) {
		Object swap = a[i];
		a[i] = a[j];
		a[j] = swap;
	}

	/***********************************************************************
	 * Check if array is sorted - useful for debugging
	 ***********************************************************************/
	private static boolean isSorted(Comparable[] a) {
		return isSorted(a, 0, a.length - 1);
	}

	private static boolean isSorted(Comparable[] a, int lo, int hi) {
		for (int i = lo + 1; i <= hi; i++)
			if (less(a[i], a[i - 1]))
				return false;
		return true;
	}

	// print array to standard output
	private static void show(Comparable[] a) {
		for (int i = 0; i < a.length; i++) {
			f5.Append(a[i] + " ");
		}
	}

	/**
	 * Reads in a sequence of strings from standard input; quicksorts them; and
	 * prints them to standard output in ascending order. Shuffles the array and
	 * then prints the strings again to standard output, but this time, using
	 * the select method.
	 */

	public static void main(String[] args) {
		File5 f5 = new File5();
		File6 f6 = new File6();
		int i = 1;
		double totalTime = 0;
		double avgTime;
		double totalTime2 = 0;
		double avgTime2;
		do {
			Comparable[] a = new Comparable[100000];
			double[] c = new double[100000];
			for (int j = 0; j < a.length; j++) {
				a[j] = (int) (Math.random() * 100);
			}
			for (int k = 0; k < c.length; k++) {
				c[k] = (int) (Math.random() * 100);

			}

			Stopwatch q1 = new Stopwatch();
			Quick.sort(a);

			totalTime += q1.elapsedTime();
			avgTime = totalTime / i;
			f6.Append("                     Trial " + i + " "
					+ q1.elapsedTime());

			Stopwatch q2 = new Stopwatch();
			Median3Partitions.sort(c);
			totalTime2 += q2.elapsedTime();
			avgTime2 = totalTime2 / i;
			f5.Append("                     Trial " + i + " "
					+ q2.elapsedTime());

			System.out.println("Processing" + i);
			i++;
		} while (i < 100000);
		f5.Append("                 The Average Time for 3 way partioning is "
				+ avgTime2);
		f6.Append("                 The Average Time for Quick Sort is "
				+ avgTime);
	}

}
