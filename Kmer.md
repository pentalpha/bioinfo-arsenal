# k-mer

> The term k-mer typically refers to all the possible substrings of length k that are contained in a string. In computational genomics, k-mers refer to all the possible subsequences (of length k) from a read obtained through DNA Sequencing. ([Wikipedia](https://en.wikipedia.org/wiki/K-mer))

## Choice of k-mer (Based on wikipedia):


### Lower k-mer sizes
- Less edges in graph;
- More overlap;
- Many vertices to the same edge. So the reconstruction of the genome becomes more difficult, because the chances of ambiguous paths is higher;
- Less information stored;
- Worst to identify repetitive DNA;

### Higher k-mer sizes
- More edges, more memory;
- Less vertices, less paths for the genome reconstruction;
- Less overlaps -> bigger risk of disjoint reads -> higher amount of small reads;
- Better for identifying repetitive regions;

## Finding k-mers:
```java
public static List<string> find_kmers(string Text, int k)
{
	var kmers = new List<string>();
	int n = Text.Length;

	for (int i = 0; i < n-k+1; i++)
	{                
	    kmers.Add(Text.Substring(i, k));               
	}

	return kmers;
}
```