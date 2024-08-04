<h1>Get_next_line</h1> <h2 align='right'>Final Grade 125/125 ✅ </h2>

The get_next_line function is designed to read and return the next line from a file descriptor. 

<h3>The prototype of get_next_line is:</h3>

```c
char *get_next_line(int fd);
```

<h2 align="center"> Description </h2>

get_next_line() maintains state between calls using a static variable, allowing it to handle file reading in a line-by-line manner. This function is particularly useful for applications that need to process input files line by line, such as text file processing, log file monitoring, or reading input from a user.

<h3>Main get_next_line Function</h3>

```c
char	*get_next_line(int fd)
{
	char		*line;
	static char	*backup;

	if (fd < 0 || BUFFER_SIZE <= 0)
		return (0);
	backup = ft_read(fd, backup);
	if (!backup)
		return (NULL);
	line = ft_get_line(backup);
	backup = ft_remove_line(backup);
	return (line);
}

```

- Static Variable: Uses a static variable backup to retain data between function calls.
- Input Validation: Checks if the file descriptor fd is valid and if BUFFER_SIZE is greater than 0.
- Read Data: Calls ft_read to read data from the file descriptor into backup.
- Extract Line: Calls ft_get_line to extract the next line from backup.
- Update Backup: Updates backup by removing the extracted line using ft_remove_line.
- Return Value: Returns the extracted line.

<h3>ft_read Function</h3>

```c
char	*ft_read(int fd, char *backup)
{
	char	*buffer;
	int		bytes;

	buffer = malloc(BUFFER_SIZE + 1);
	if (!buffer)
		return (NULL);
	bytes = 1;
	while (!ft_strchr(backup, '\n') && bytes != 0)
	{
		bytes = read(fd, buffer, BUFFER_SIZE);
		if (bytes == -1)
		{
			free(buffer);
			return (NULL);
		}
		buffer[bytes] = '\0';
		backup = ft_strjoin(backup, buffer);
	}
	free(buffer);
	return (backup);
}
```
- Purpose: Reads data from the file descriptor fd into the backup string until a newline character (\n) is encountered or the end of the file is reached.
- Buffer Allocation: Allocates memory for a buffer to temporarily hold data read from the file.
- Reading Loop: Continuously reads from the file descriptor in chunks of size BUFFER_SIZE until a newline character is found or no more data is available.
- Error Handling: If a read error occurs, it frees the buffer and returns NULL.
- String Join: Appends the newly read data to the backup string using a helper function ft_strjoin.
- Return Value: Returns the updated backup string containing the newly read data.

<h3>ft_get_line Function</h3>

```c
char	*ft_get_line(char *s1)
{
	int		i;
	char	*new;

	i = 0;
	if (!s1[i])
		return (NULL);
	while (s1[i] && s1[i] != '\n')
		i++;
	new = (char *)malloc(sizeof(char) * (i + 2));
	if (!new)
		return (NULL);
	i = 0;
	while (s1[i] && s1[i] != '\n')
	{
		new[i] = s1[i];
		i++;
	}
	if (s1[i] == '\n')
	{
		new[i] = s1[i];
		i++;
	}
	new[i] = '\0';
	return (new);
}

```

- Purpose: Extracts the next line from the backup string.
- Line Length Calculation: Iterates through the backup string to determine the length of the line up to and including the newline character.
- Memory Allocation: Allocates memory for the new line string.
- String Copy: Copies characters from the backup string to the new line string until the newline character or end of string is reached.
- Return Value: Returns the new line string.

<h3>ft_remove_line Function</h3>

```c
char	*ft_remove_line(char *s1)
{
	int		i;
	int		j;
	char	*new;

	i = 0;
	while (s1[i] && s1[i] != '\n')
		i++;
	if (!s1[i])
	{
		free(s1);
		return (NULL);
	}
	new = (char *)malloc(sizeof(char) * (ft_strlen(s1) - i + 1));
	if (!new)
		return (NULL);
	i++;
	j = 0;
	while (s1[i])
		new[j++] = s1[i++];
	new[j] = '\0';
	free(s1);
	return (new);
}

```
- Purpose: Removes the extracted line from the input string, effectively updating the string to contain only the data after the newline character.
- Find Newline: Iterates through s1 to find the newline character.
- Check End of String: If no newline character is found, it frees s1 and returns NULL, indicating there's no more data to process.
- Allocate Memory: Allocates memory for the new string, which will hold the data after the newline character.
- Copy Data: Copies characters from the original string s1 to the new string starting after the newline character.
- Free Original String: Frees the memory allocated for the original string s1.
- Return Value: Returns the new string containing the remaining data.

<h3>Example Usage</h3>

```c

#include "get_next_line.h"

int main()
{
    int fd = open("example.txt", O_RDONLY);
    char *line;

    while ((line = get_next_line(fd)) != NULL)
    {
        printf("%s\n", line);
        free(line);
    }
    close(fd);
    return 0;
}

```








































