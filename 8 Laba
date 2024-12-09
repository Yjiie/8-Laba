''' Требуется написать ООП с графическим интерфейсом в соответствии со своим вариантом. 
Должны быть реализованы минимум один класс, три атрибута, четыре метода (функции). 
Ввод данных из файла с контролем правильности ввода. 
Базы данных не использовать. При необходимости сохранять информацию в файлах, разделяя значения запятыми (CSV файлы) или пробелами. Для GUI использовать библиотеку tkinter.

Вариант Киселев Андрей
Объекты – билеты на концерт 
Функции:	сегментация полного списка билетов по зонам (ценовым)
визуализация предыдущей функции в форме столбиковой диаграммы
сегментация списка продавцов по выручке
визуализация предыдущей функции в форме круговой диаграммы
'''
import tkinter as tk
from tkinter import filedialog, messagebox
import csv
from collections import defaultdict
import matplotlib.pyplot as plt

class ConcertTickets:
    def __init__(self, file_path):
        self.file_path = file_path
        self.tickets = []
        self.load_tickets()

    def load_tickets(self):
        """Load tickets from a CSV file with validation."""
        try:
            with open(self.file_path, 'r', encoding='utf-8') as file:
                reader = csv.DictReader(file)
                for row in reader:
                    # Normalize headers to handle both English and Russian
                    zone_key = "Zone" if "Zone" in row else "Зона"
                    price_key = "Price" if "Price" in row else "Цена"
                    seller_key = "Seller" if "Seller" in row else "Продавец"

                    if zone_key in row and price_key in row and seller_key in row:
                        self.tickets.append({
                            "Zone": row[zone_key],
                            "Price": float(row[price_key]),
                            "Seller": row[seller_key],
                        })
                    else:
                        raise ValueError("Неверный формат файла")
        except Exception as e:
            messagebox.showerror("Ошибка", f"Не удалось загрузить билеты: {e}")

    def segment_by_zone(self):
        """Segment tickets by zones and calculate counts."""
        zone_counts = defaultdict(int)
        for ticket in self.tickets:
            zone_counts[ticket["Zone"]] += 1
        return zone_counts

    def segment_by_seller(self):
        """Segment sellers by total revenue."""
        seller_revenue = defaultdict(float)
        for ticket in self.tickets:
            seller_revenue[ticket["Seller"]] += ticket["Price"]
        return seller_revenue

    def visualize_zones(self):
        """Visualize ticket segmentation by zones."""
        zone_counts = self.segment_by_zone()
        zones = list(zone_counts.keys())
        counts = list(zone_counts.values())

        plt.bar(zones, counts, color='skyblue')
        plt.title("Сегментация билетов по зонам")
        plt.xlabel("Зона")
        plt.ylabel("Количество билетов")
        plt.show()

    def visualize_sellers(self):
        """Visualize seller revenue as a pie chart."""
        seller_revenue = self.segment_by_seller()
        sellers = list(seller_revenue.keys())
        revenue = list(seller_revenue.values())

        plt.pie(revenue, labels=sellers, autopct="%1.1f%%", startangle=140)
        plt.title("Доходы по продавцам")
        plt.show()

class TicketApp:
    def __init__(self, root):
        self.root = root
        self.root.title("Менеджер билетов на концерт")
        self.root.geometry("400x300")
        self.root.configure(bg="#f0f8ff")

        self.file_path = None
        self.tickets_manager = None

        # GUI Elements
        self.label = tk.Label(root, text="Загрузите CSV файл для начала работы", bg="#f0f8ff", font=("Helvetica", 12))
        self.label.pack(pady=20)

        self.load_button = tk.Button(root, text="Загрузить билеты", command=self.load_file, bg="#4682b4", fg="white", font=("Helvetica", 10, "bold"))
        self.load_button.pack(pady=10)

        self.zone_button = tk.Button(root, text="Сегментировать по зонам", command=self.segment_zones, state=tk.DISABLED, bg="#5f9ea0", fg="white", font=("Helvetica", 10, "bold"))
        self.zone_button.pack(pady=10)

        self.seller_button = tk.Button(root, text="Сегментировать по продавцам", command=self.segment_sellers, state=tk.DISABLED, bg="#5f9ea0", fg="white", font=("Helvetica", 10, "bold"))
        self.seller_button.pack(pady=10)

    def load_file(self):
        """Load a file and initialize ticket manager."""
        self.file_path = filedialog.askopenfilename(filetypes=[("CSV файлы", "*.csv")])
        if not self.file_path:
            return

        self.tickets_manager = ConcertTickets(self.file_path)
        if self.tickets_manager.tickets:
            self.label.config(text=f"Загружено {len(self.tickets_manager.tickets)} билетов")
            self.zone_button.config(state=tk.NORMAL)
            self.seller_button.config(state=tk.NORMAL)
        else:
            self.label.config(text="Не удалось загрузить билеты")

    def segment_zones(self):
        """Visualize segmentation by zones."""
        if self.tickets_manager:
            self.tickets_manager.visualize_zones()

    def segment_sellers(self):
        """Visualize segmentation by sellers."""
        if self.tickets_manager:
            self.tickets_manager.visualize_sellers()

if __name__ == "__main__":
    root = tk.Tk()
    app = TicketApp(root)
    root.mainloop()
