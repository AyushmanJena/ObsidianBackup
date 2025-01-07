Read an image from a file
```java
BufferedImage image = ImageIO.read(new File("c:\\test\\image.png"));
```

Read an Image from an URL
```java
BufferedImage image = ImageIO.read(new URL("https://example.com/image.png"));
```

Write or save an Image
```java
ImageIO.write(bufferedImage , "jpg", new File("c:\\test\\image.jpg")); ImageIO.write(bufferedImage , "gif", new File("c:\\test\\image.gif")); ImageIO.write(bufferedImage , "png", new File("c:\\test\\image.png"));
```

Writing or adding text to an existing image
```java
import javax.imageio.ImageIO;
import java.awt.*;
import java.awt.image.BufferedImage;
import java.io.File;

public class test{

    public static void main(String[] args) throws Exception{
        System.out.println("start");

        try{
            final BufferedImage image = ImageIO.read(new File("test.png"));

            Graphics g = image.getGraphics();
            g.setFont(g.getFont().deriveFont(30f));
            g.drawString("Hello World!", 100, 100);
            g.dispose();
            ImageIO.write(image, "png", new File("quote.png"));
            System.out.println("written");

        }
        catch(Exception e){
            System.out.println("didn't work");
        }

        System.out.println("end");

    }

}

```