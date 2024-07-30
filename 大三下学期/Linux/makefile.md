```c
TARGET = hello
CXX = g++
OBJ = main.o print.o

CXXFLAGS = -c -Wall

$(TARGET) : $(OBJ)
	$(CXX) -o $@ @^

%.o : %.c
	$(CXX) $(CXXFLAGS) $< -o $@

.PHONY:clean
clean:
	rm -rf *.o $(TARGET) 
```



