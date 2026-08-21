### EX4 Implementation of Cluster and Visitor Segmentation for Navigation patterns
### DATE: 21/08/2026
### AIM: To implement Cluster and Visitor Segmentation for Navigation patterns in Python.
### Description:
<div align= "justify">Cluster visitor segmentation refers to the process of grouping or categorizing visitors to a website, 
  application, or physical location into distinct clusters or segments based on various characteristics or behaviors they exhibit. 
  This segmentation allows businesses or organizations to better understand their audience and tailor their strategies, marketing efforts, 
  or services to meet the specific needs and preferences of each cluster.</div>
  
### Procedure:
1) Read the CSV file: Use pd.read_csv to load the CSV file into a pandas DataFrame.
2) Define Age Groups by creating a dictionary containing age group conditions using Boolean conditions.
3) Segment Visitors by iterating through the dictionary and filter the visitors into respective age groups.
4) Visualize the result using matplotlib.

### Program:
```
import pandas as pd
import matplotlib.pyplot as plt

# Read CSV file
df = pd.read_csv("/content/clustervisitor.csv")

# Display the visitor dataset
print("Visitor Dataset:")
print(df)

# Select the Age feature
ages = df["Age"].tolist()

# Number of clusters
K = 3

# Initial centroids
centroids = [
    min(ages),
    sum(ages) / len(ages),
    max(ages)
]

# Repeat clustering process
for iteration in range(10):

    clusters = [[], [], []]

    # Assign each visitor to the nearest centroid
    for age in ages:

        distances = [
            abs(age - centroid)
            for centroid in centroids
        ]

        nearest_cluster = distances.index(
            min(distances)
        )

        clusters[nearest_cluster].append(age)

    # Calculate new centroids
    new_centroids = []

    for i in range(K):

        if len(clusters[i]) > 0:

            new_centroid = (
                sum(clusters[i])
                / len(clusters[i])
            )

        else:

            new_centroid = centroids[i]

        new_centroids.append(new_centroid)

    # Stop when centroids do not change
    if new_centroids == centroids:
        break

    centroids = new_centroids


# Assign final cluster labels to visitors
visitor_clusters = []

for age in ages:

    distances = [
        abs(age - centroid)
        for centroid in centroids
    ]

    nearest_cluster = distances.index(
        min(distances)
    )

    visitor_clusters.append(
        nearest_cluster
    )


# Add cluster labels to the DataFrame
df["Cluster"] = visitor_clusters


# Display visitor details with cluster labels
print("\nVisitor Details with Clusters:")
print(df)


# Display cluster-wise visitor details
for i in range(K):

    print(f"\nVisitors in Cluster {i}:")
    print(df[df["Cluster"] == i])


# Display final centroids
print("\nFinal Centroids:")

for i in range(K):

    print(f"Cluster {i}: {centroids[i]:.2f}")


plt.figure(figsize=(10,6))

for i in range(K):

    cluster_data = df[
        df["Cluster"] == i
    ]

    # Plot visitor points
    plt.scatter(
        cluster_data["Age"],
        cluster_data["Cluster"],
        label=f"Cluster {i}",
        s=100
    )

    # Display age above each point
    for _, row in cluster_data.iterrows():

        plt.annotate(
            str(row["Age"]),
            (
                row["Age"],
                row["Cluster"]
            ),
            textcoords="offset points",
            xytext=(0, 10),
            ha="center"
        )

# Axis labels and title
plt.xlabel("Age")
plt.ylabel("Cluster")
plt.title("Visitor Segmentation using K-Means")

# Show normal X and Y axes
ax = plt.gca()
ax.spines['left'].set_visible(True)
ax.spines['bottom'].set_visible(True)
ax.spines['left'].set_linewidth(1.5)
ax.spines['bottom'].set_linewidth(1.5)

plt.legend()
plt.grid(True)
plt.show()

```
### Output:
<img width="382" height="572" alt="image" src="https://github.com/user-attachments/assets/15875abd-4718-4250-bf42-9d20679706cb" />

<img width="431" height="575" alt="image" src="https://github.com/user-attachments/assets/324177ef-8a4f-452b-91f5-87099d108f01" />

<img width="427" height="677" alt="image" src="https://github.com/user-attachments/assets/32aca3a5-c23d-4ee3-9e13-0806c7402eae" />
<img width="1037" height="636" alt="image" src="https://github.com/user-attachments/assets/58408af6-430a-4a0d-9c9b-c48aaf0e3496" />


### Result:

Thus, Cluster and Visitor Segmentation for Navigation Patterns was successfully implemented in Python, and the visitor distribution across different age groups was visualized successfully using a bar chart.
