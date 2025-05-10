#code alpha STOCK-TRADING
It's my task-2
import java.util.*;

public class StockTradingPlatform {

    static class Stock {
        private final String symbol;
        private double price;

        public Stock(String symbol, double price) {
            this.symbol = symbol;
            this.price = price;
        }

        public String getSymbol() {
             return symbol;
        }

        public double getPrice() {
            return price;
        }

        public void updatePrice() {
            double change = (Math.random() - 0.5) * 0.1; // -5% to +5%
            price *= (1 + change);
            price = Math.round(price * 100.0) / 100.0;
        }
    }
static class Market {
        private final Map<String, Stock> stocks = new HashMap<>();

        public Market() {
            stocks.put("AAPL", new Stock("AAPL", 6010));
            stocks.put("GOOGL", new Stock("GOOGL", 2870));
            stocks.put("TSLA", new Stock("TSLA", 5980));
            stocks.put("AMZN", new Stock("AMZN", 3840));
        }

        public void updateMarket() {
            for (Stock stock : stocks.values()) {
                stock.updatePrice();
            }
        }
        public void showMarket() {
            System.out.println("\n--- Market Prices ---");
            for (Stock stock : stocks.values()) {
                System.out.printf("%s: $%.2f%n", stock.getSymbol(), stock.getPrice());
            }
            }

        public Stock getStock(String symbol) {
            return stocks.get(symbol.toUpperCase());
        }

        public Set<String> getAvailableSymbols() {
            return stocks.keySet();
        }
    }

    static class Portfolio {
        private double cash = 6000;
        private final Map<String, Integer> holdings = new HashMap<>();

        public boolean buy(String symbol, int qty, double price) {
            double cost = qty * price;
            if (cash >= cost) {
                cash -= cost;
                holdings.put(symbol, holdings.getOrDefault(symbol, 0) + qty);
                return true;
            }
            return false;
        }

        public boolean sell(String symbol, int qty, double price) {
            int currentQty = holdings.getOrDefault(symbol, 0);
            if (currentQty >= qty) {
                holdings.put(symbol, currentQty - qty);
                cash += qty * price;
                return true;
            }
            return false;
        }
        public void showPortfolio(Market market) {
            System.out.println("\n--- Portfolio ---");
            System.out.printf("Cash: $%.2f%n", cash);
            for (String symbol : holdings.keySet()) {
                int qty = holdings.get(symbol);
                double price = market.getStock(symbol).getPrice();
                double value = qty * price;
                System.out.printf("%s: %d shares | Value: $%.2f%n", symbol, qty, value);
            }
        }
    }

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        Market market = new Market();
        Portfolio portfolio = new Portfolio();

        while (true) {
            market.updateMarket();
            market.showMarket();
            portfolio.showPortfolio(market);

            System.out.println("\nChoose an action: [buy] [sell] [exit]");
            String action = scanner.nextLine().trim().toLowerCase();

            if (action.equals("exit")) break;

            System.out.print("Enter stock symbol: ");
            String symbol = scanner.nextLine().trim().toUpperCase();

            if (!market.getAvailableSymbols().contains(symbol)) {
                System.out.println("Invalid stock symbol.");
                continue;
            }

            System.out.print("Enter quantity: ");
            int qty;
            try {
                qty = Integer.parseInt(scanner.nextLine().trim());
            } catch (Exception e) {
                System.out.println("Invalid quantity.");
                continue;
            }

            Stock stock = market.getStock(symbol);
            boolean success = false;

            if (action.equals("buy")) {
                success = portfolio.buy(symbol, qty, stock.getPrice());
            } else 
            if (action.equals("sell")) {
                success = portfolio.sell(symbol, qty, stock.getPrice());
            }

            if (success) {
                System.out.println("Transaction successful.");
            } else {System.out.println("Transaction failed. Check your balance or holdings.");
            }
        }

        System.out.println("Thanks for using the stock trading simulator!");
        scanner.close();
    }
}

output :
--- Market Prices ---
GOOGL: $2754.03
AAPL: $6006.50
TSLA: $6036.63
AMZN: $3932.39

--- Portfolio ---
Cash: $6000.00

Choose an action: [buy] [sell] [exit]
BUY
Enter stock symbol: AAPL
Enter quantity: 30
Transaction failed. Check your balance or holdings.

--- Market Prices ---
GOOGL: $2807.68
AAPL: $5936.45
TSLA: $6305.83
AMZN: $3898.28

--- Portfolio ---
Cash: $6000.00

Choose an action: [buy] [sell] [exit]

Enter stock symbol: SELL
Invalid stock symbol.
--- Market Prices ---
GOOGL: $2884.44
AAPL: $5707.78
TSLA: $6152.50
AMZN: $4015.17

--- Portfolio ---
Cash: $6000.00

Choose an action: [buy] [sell] [exit]

Enter stock symbol: GOOGL
Enter quantity: 50
Transaction failed. Check your balance or holdings.

--- Market Prices ---
GOOGL: $2951.18
AAPL: $5779.71
TSLA: $5916.91
AMZN: $3855.38

--- Portfolio ---
Cash: $6000.00

Choose an action: [buy] [sell] [exit]
XIT
Thanks for using the stock trading simulator!

=== Code Execution Successful ===
