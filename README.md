import static org.junit.jupiter.api.Assertions.*;

import java.io.ByteArrayInputStream;
import java.io.InputStream;
import java.io.PrintStream;

import org.junit.jupiter.api.*;
import org.junit.jupiter.api.MethodOrderer.OrderAnnotation;
import java.io.ByteArrayOutputStream;
import java.lang.reflect.Method;


@TestMethodOrder(OrderAnnotation.class)
class TestFile {

    // IO Streams
    private static final  ByteArrayOutputStream out = new ByteArrayOutputStream();
    private static final ByteArrayOutputStream err = new ByteArrayOutputStream();
    private static ByteArrayInputStream inputStream;

    // Backup Streams
    private static final PrintStream originalOut = System.out;
    private static final PrintStream originalErr = System.err;
    private static final InputStream originalInput = System.in;


    @BeforeAll
    public static void setStreams() { // Override all the streams to our custom IO streams
        System.setOut(new PrintStream(out));
        System.setErr(new PrintStream(err));
    }

    @AfterAll
    public static void restoreInitialStreams() { // Restores all the streams to its original backups
        System.setOut(originalOut);
        System.setErr(originalErr);
        System.setIn(originalInput);
    }


    public static void provideInput(String data) { // Used to give and test custom inputs (Use-full for Scanner class)
        inputStream = new ByteArrayInputStream(data.getBytes());
        System.setIn(inputStream);
    }

    @Test
    @Order(1)
    public void test1() {
        try {

            provideInput("3\n4\n5\n"); // Give multiple inputs to the user
            
            Class classNameToTest = Class.forName("<Class Name Here>");
            Method methodToTest = classNameToTest.getMethod("< Method Name Here >"); // Optional: Add parameter types as second argument here

            methodToTest.invoke(null, null); // Give arguments as an array in 2nd parameter

            assertTrue(true); // Check for the expected output

        } catch(Exception e) {
            fail();
        }
    }
}
