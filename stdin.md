# How to read from standard input

The standard input(stdin) can be represented by System.in in Java. The System.in is an instance of the InputStream class. It means that all its methods work on bytes, not Strings. To read any data from a keyboard, we can use either a **Reader** class or **Scanner** class.

## BufferReader Example #1
```Java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

public class BufferedReaderExample {
    public static void main(String[] args) throws IOException {
        BufferedReader reader = new BufferedReader(new InputStreamReader(System.in));

        // Reading a line of text
        System.out.print("Enter a line: ");
        String line = reader.readLine();
        System.out.println("You entered: " + line);

        // Reading an integer
        System.out.print("Enter an integer: ");
        int num = Integer.parseInt(reader.readLine());
        System.out.println("You entered: " + num);

        // Reading a double
        System.out.print("Enter a double: ");
        double dbl = Double.parseDouble(reader.readLine());
        System.out.println("You entered: " + dbl);

        // Don't forget to close it
        reader.close();
    }
}
```

## Scanner Example #1
```Java
import java.util.Scanner;

public class ScannerExample {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        // Reading a line of text
        System.out.print("Enter a line: ");
        String line = scanner.nextLine();
        System.out.println("You entered: " + line);

        // Reading an integer
        System.out.print("Enter an integer: ");
        int num = scanner.nextInt();
        System.out.println("You entered: " + num);

        // Reading a double
        System.out.print("Enter a double: ");
        double dbl = scanner.nextDouble();
        System.out.println("You entered: " + dbl);
        
        // Don't forget to close it
        scanner.close();
    }
}
```

## BufferReader Example #2 (with while loop)
```Java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

public class BufferedReaderWhileLoopExample {
    public static void main(String[] args) throws IOException {
        BufferedReader reader = new BufferedReader(new InputStreamReader(System.in));

        System.out.println("Enter lines of text (type 'exit' to stop):");
        String line;
        while ((line = reader.readLine()) != null && !line.equalsIgnoreCase("exit")) {
            System.out.println("You entered: " + line);
        }

        reader.close();
    }
}
```

## Scanner Example #2 (with while loop)
```Java
import java.util.Scanner;

public class ScannerWhileLoopExample {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        System.out.println("Enter lines of text (type 'exit' to stop):");
        while (scanner.hasNextLine()) {
            String line = scanner.nextLine();
            if (line.equalsIgnoreCase("exit")) {
                break;
            }
            System.out.println("You entered: " + line);
        }

        scanner.close();
    }
}
```

# Conclusion
Both BufferedReader and Scanner can be used to read input, but they have different features and performance characteristics. BufferedReader is more efficient for reading large amounts of text, while Scanner provides more convenient methods for parsing different data types. Choose the one that suits your specific needs and the type of input you're working with.
