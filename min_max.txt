#include <iostream>
#include <omp.h>
#include <climits>
using namespace std;
void min_reduction(int arr[], int n)
{
  int min_value = INT_MAX;
#pragma omp parallel for reduction(min : min_value)
  for (int i = 0; i < n; i++)
  {
    if (arr[i] < min_value)
    {
      min_value = arr[i];
    }
  }
  cout << "Minimum value: " << min_value << endl;
}

void max_reduction(int arr[], int n)
{
  int max_value = INT_MIN;
#pragma omp parallel for reduction(max : max_value)
  for (int i = 0; i < n; i++)
  {
    if (arr[i] > max_value)
    {
      max_value = arr[i];
    }
  }
  cout << "Maximum value: " << max_value << endl;
}

void sum_reduction(int arr[], int n)
{
  int sum = 0;
#pragma omp parallel for reduction(+ : sum)
  for (int i = 0; i < n; i++)
  {
    sum += arr[i];
  }
  cout << "Sum: " << sum << endl;
}

void average_reduction(int arr[], int n)
{
  int sum = 0;
#pragma omp parallel for reduction(+ : sum)
  for (int i = 0; i < n; i++)
  {
    sum += arr[i];
  }
  cout << "Average: " << (double)sum / (n) << endl;
}

void printArray(int arr[], int n)
{
  for (int i = 0; i < n; i++)
    cout << arr[i] << " ";
  cout << "\n";
}

int main()
{
  int *arr, n;
  double start_time, end_time;
  cout << "Enter total no of elements=> ";
  cin >> n;
  arr = new int[n];
  cout << "\nEnter elements=> ";
  for (int i = 0; i < n; i++)
  {
    cin >> arr[i];
  }
  cout << "Array is\n";
  printArray(arr, n);

  min_reduction(arr, n);
  max_reduction(arr, n);
  sum_reduction(arr, n);
  average_reduction(arr, n);

  start_time = omp_get_wtime();
  min_reduction(arr, n);
  end_time = omp_get_wtime();
  cout << "\nParallel Minimum value: " << end_time - start_time << " seconds.\n";

  start_time = omp_get_wtime();
  max_reduction(arr, n);
  end_time = omp_get_wtime();
  cout << "Parallel Maximum value time: " << end_time - start_time << " seconds.\n";

  start_time = omp_get_wtime();
  sum_reduction(arr, n);
  end_time = omp_get_wtime();
  cout << "Parallel Sum value time: " << end_time - start_time << " seconds.\n";

  start_time = omp_get_wtime();
  average_reduction(arr, n);
  end_time = omp_get_wtime();
  cout << "Parallel Average value time: " << end_time - start_time << " seconds.\n";
}
