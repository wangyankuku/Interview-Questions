package test;

import java.util.*;
import java.util.concurrent.BlockingQueue;
import java.util.concurrent.LinkedBlockingQueue;

public class ThreadPool extends Thread {
	private final BlockingQueue < Runnable > workQueue;
	private final Thread[] workerThreads;
	Worker worker = new Worker();
	boolean shutdown = false;

	public ThreadPool(int n) {
		workQueue = new LinkedBlockingQueue < Runnable > ();
		workerThreads = new Thread[n];
	}

	public void startAllThreads() {
		for (int i = 0; i < workerThreads.length; i++) {
			workerThreads[i] = new Thread(worker, "thread -- " + i);
			workerThreads[i].start();
		}
	}

	public void addTask(Runnable task) {
		try {
			workQueue.put(task);
		} catch (InterruptedException e) {
			// TODO Auto-generated catch block
			Thread.currentThread().interrupt();
		}
	}

	public void terminate() {
		while (!workQueue.isEmpty()) {
			try {
				Thread.sleep(10);
			} catch (InterruptedException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			}
		}

		shutdown = true;

		for (Thread thread: workerThreads) {
			if (!thread.getState().equals(Thread.State.RUNNABLE)) {
				// System.out.println(thread.getState());
				thread.interrupt();
			}
		}
	}

	private class Worker implements Runnable {
		public void run() {
			while (!shutdown) {

				try {
					Runnable cur = workQueue.take();
					cur.run();
				} catch (InterruptedException e) {
					// TODO Auto-generated catch block
				}

			}
		}
	}

	public static void main(String[] args) {
		Test ttt = new Test();

		ThreadPool tp = new ThreadPool(200);

		for (int i = 0; i < 10; i++) {
			tp.addTask(ttt);
		}
		tp.startAllThreads();
		tp.terminate();
	}
}

class Test implements Runnable {
	int n = 10000;

	public void run() {

		while (true) {
			synchronized(this) {
				if (n-- > 0) System.out.println(Thread.currentThread().getName() + " -- " + n);
				else break;
			}
		}
	}
}
