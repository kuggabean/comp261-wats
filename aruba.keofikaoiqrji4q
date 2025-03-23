import java.util.*;
import java.util.regex.Pattern;


interface HTML {}

record Text(String content) implements HTML {
    public String toString() { return content; }
}

record Div(List<HTML> children) implements HTML {
    public String toString() { return "<DIV>" + children + "</DIV>"; }
}

record UL(List<LI> items) implements HTML {
    public String toString() { return "<UL>" + items + "</UL>"; }
}

record LI(HTML content) {
    public String toString() { return "<LI>" + content + "</LI>"; }
}


class Parser {
    private final LinkedList<Token> tokens;
    public Parser(List<Token> tokens) { this.tokens = new LinkedList<>(tokens); }

    public HTML parseAll() {
        if (tokens.isEmpty()) throw new RuntimeException();
        HTML result = parseHTML();
        if (!tokens.isEmpty()) throw new RuntimeException();
        return result;
    }

    private HTML parseHTML() {
        if (tokens.isEmpty()) throw new RuntimeException("aaa");
        Token first = tokens.peek();

        return switch (first.kind().name()) {
            case "DivOpen" -> parseDiv();
            case "UlOpen" -> parseUL();
            case "TextT" -> new Text(tokens.removeFirst().content());
            default -> throw new RuntimeException();
        };
    }

    private Div parseDiv() {
        tokens.removeFirst();
        List<HTML> children = new ArrayList<>();
        while (!tokens.isEmpty() && !tokens.peek().kind().name().equals("DivClosed")) {
            children.add(parseHTML());
        }
        expect("DivClosed");
        return new Div(children);
    }

    private UL parseUL() {
        tokens.removeFirst();
        List<LI> items = new ArrayList<>();

        if (tokens.isEmpty() || !tokens.peek().kind().name().equals("LiOpen")) {
            throw new RuntimeException();
        }

        while (!tokens.isEmpty() && tokens.peek().kind().name().equals("LiOpen")) {
            items.add(parseLI());
        }

        expect("UlClosed");
        return new UL(items);
    }

    private LI parseLI() {
        tokens.removeFirst();
        HTML content = parseHTML();
        expect("LiClosed");
        return new LI(content);
    }

    private void expect(String kind) {
        if (tokens.isEmpty() || !tokens.peek().kind().name().equals(kind)) {
            throw new RuntimeException();
        }
        tokens.removeFirst();
    }

    public static Tokenizer initTokenizer() {
        return new Tokenizer()
                .add("DivOpen", "<DIV>", false)
                .add("DivClosed", "</DIV>", false)
                .add("UlOpen", "<UL>", false)
                .add("UlClosed", "</UL>", false)
                .add("LiOpen", "<LI>", false)
                .add("LiClosed", "</LI>", false)
                .add("TextT", "[^<>]+", false);
    }
}
