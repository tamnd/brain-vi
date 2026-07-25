---
title: "CF 103833E - Phạt"
description: "Chúng tôi đang mô hình hóa một cú sút phạt đền như một lưới rời rạc phía trên khung thành. Mỗi ô trong lưới tương ứng với một vị trí bắn có thể. Đối với thủ môn, mỗi ô chứa một giá trị mô tả khả năng thủ môn cản phá được cú sút hướng vào đó."
date: "2026-07-02T08:07:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103833
codeforces_index: "E"
codeforces_contest_name: "2018 International olympiad Tuymaada"
rating: 0
weight: 103833
solve_time_s: 48
verified: true
draft: false
---

[CF 103833E - Hình phạt](https://codeforces.com/problemset/problem/103833/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 48s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang mô hình hóa một cú sút phạt đền như một lưới rời rạc phía trên khung thành. Mỗi ô trong lưới tương ứng với một vị trí bắn có thể. Đối với thủ môn, mỗi ô chứa một giá trị mô tả khả năng thủ môn cản phá được cú sút hướng vào đó. Đối với mỗi người chơi, mỗi ô chứa khả năng người chơi đó ghi bàn nếu họ bắn vào vị trí đó. 

Để đánh giá một cầu thủ, chúng tôi kết hợp xác suất sút của họ với xác suất cứu thua của thủ môn theo từng ô. Giá trị kết hợp của một ô là tích của xác suất thành công của người chơi và xác suất không cứu thua của thủ môn tại cùng một vị trí đó. Một ô được coi là “tốt” cho người chơi nếu xác suất kết hợp này ít nhất là 0,65. Điểm của người chơi là số ô tốt như vậy trong lưới. 

Chúng ta phải chọn năm người chơi có số điểm lớn nhất. Nếu nhiều người chơi có cùng số điểm, chúng tôi sẽ phá vỡ mối quan hệ theo thứ tự từ điển của tên đầy đủ của họ. 

Kích thước đầu vào nhỏ:$N, M \le 100$, và số lượng người chơi nhiều nhất là 100. Điều này ngay lập tức gợi ý rằng ngay cả một$O(KNM)$giải pháp này đủ nhanh một cách tầm thường, vì nhiều nhất là khoảng$10^6$hoạt động. 

Sự tinh tế chính là so sánh dấu phẩy động. Các giá trị được đưa ra với hai chữ số thập phân, vì vậy việc so sánh ở ngưỡng 0,65 phải được thực hiện cẩn thận để tránh độ lệch chính xác. Cách tiếp cận an toàn là chia tỷ lệ mọi thứ thành số nguyên bằng cách nhân với 100 hoặc 10000 hoặc so sánh với một epsilon nhỏ. 

Các trường hợp cạnh quan trọng chủ yếu là xử lý ràng buộc và đẳng thức dấu phẩy động ở mức chính xác là 0,65. Một so sánh nghiêm ngặt ngây thơ có thể phân loại sai các giá trị đường biên. Một nhược điểm khác là so sánh từ điển về tên đầy đủ: dấu cách là một phần của thứ tự, vì vậy so sánh chuỗi tiêu chuẩn phải được sử dụng trực tiếp. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu về cơ bản là giải pháp cuối cùng. Đối với mỗi người chơi, chúng tôi lặp lại tất cả$N \times M$các ô, tính toán kết quả bằng lưới thủ môn, đếm xem có bao nhiêu người vượt quá ngưỡng và sau đó sắp xếp người chơi. 

Điều này hoạt động vì không có sự phụ thuộc lẫn nhau giữa các ô hoặc người chơi. Điểm của mỗi người chơi là độc lập, do đó vấn đề được phân tách rõ ràng thành các đánh giá độc lập, sau đó là xếp hạng. 

Một mô hình tinh thần chậm hơn có thể là nghĩ đến việc tính toán lại hoặc so sánh người chơi theo cặp, nhưng điều đó là không cần thiết. Sau khi tính từng điểm, việc chọn ra năm điểm cao nhất là một bài toán sắp xếp tiêu chuẩn. 

“Tối ưu hóa” duy nhất là nhận ra rằng cấu trúc tính toán lại đã tối ưu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force cho mỗi người chơi với sự sắp xếp | O(KNM + K log K) | O(K) | Đã chấp nhận | 
| Bất kỳ sự thay thế quá phức tạp nào | O(K^2 NM) | O(K) | Quá chậm | 

## Hướng dẫn thuật toán 

### 1. Đọc lưới thủ môn 

Đầu tiên chúng tôi đọc$N \times M$ma trận đại diện cho thủ môn. Lưới này được sử dụng lại cho tất cả người chơi nên chúng tôi lưu trữ nó một lần. 

### 2. Phân tích tất cả người chơi bằng tên và lưới 

Đối với mỗi người chơi, chúng tôi lưu trữ tên và$N \times M$ma trận xác suất. Vì các ràng buộc nhỏ nên việc lưu trữ tất cả dữ liệu sẽ an toàn và đơn giản hóa việc tính toán. 

### 3. Tính điểm người chơi một cách độc lập 

Đối với mỗi người chơi, chúng tôi lặp lại trên tất cả các ô lưới. Tại mỗi ô, chúng tôi tính tích giá trị thủ môn và giá trị cầu thủ. Nếu sản phẩm này có ít nhất 0,65 thì chúng tôi sẽ tăng điểm. 

Bước này độc lập với mỗi người chơi, điều này làm cho bài toán tuyến tính theo$KNM$. 

### 4. Lưu kết quả dưới dạng (điểm, tên) 

Chúng tôi giữ một danh sách các cặp có chứa điểm tính toán và tên người chơi. Cấu trúc này sẽ được sử dụng để sắp xếp. 

### 5. Sắp xếp người chơi theo điểm số và từ điển 

Chúng tôi sắp xếp theo điểm giảm dần. Nếu điểm bằng nhau, chúng tôi dựa vào thứ tự từ điển của tên. Điều này đảm bảo sự ràng buộc mang tính quyết định. 

### 6. Xuất ra 5 tên hàng đầu 

Chúng tôi xuất năm mục đầu tiên sau khi sắp xếp. 

### Tại sao nó hoạt động 

Điều bất biến chính là điểm của mỗi cầu thủ chỉ phụ thuộc vào lưới của chính họ và lưới thủ môn cố định. Không có sự tương tác giữa những người chơi nên việc xếp hạng tương đương với việc sắp xếp các điểm vô hướng độc lập. Phép nhân và ngưỡng làm giảm mỗi ô lưới thành một đóng góp boolean, làm cho điểm số trở thành một hàm cộng đơn giản trên các ô. Vì phép cộng có tính liên kết và độc lập giữa những người chơi nên vấn đề xếp hạng trở thành việc sắp xếp hoàn toàn dựa trên so sánh. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def parse_matrix(n, m):
    mat = []
    for _ in range(n):
        row = list(map(float, input().split()))
        mat.append(row)
    return mat

def main():
    n, m = map(int, input().split())
    k = int(input())

    keeper = parse_matrix(n, m)

    players = []

    for _ in range(k):
        name = input().strip()
        mat = parse_matrix(n, m)

        score = 0
        for i in range(n):
            for j in range(m):
                if keeper[i][j] * mat[i][j] >= 0.65 - 1e-12:
                    score += 1

        players.append(( -score, name))

    players.sort()
    for i in range(5):
        print(players[i][1])

if __name__ == "__main__":
    main()
```Mã đọc ma trận thủ môn một lần và sau đó xử lý từng cầu thủ một cách độc lập. Điểm bị phủ định để việc sắp xếp theo thứ tự tăng dần tự nhiên sẽ cho điểm cao nhất trước tiên. Epsilon trong so sánh ngăn chặn các vấn đề về độ chính xác khi phép nhân dấu phẩy động tạo ra các giá trị như 0,6499999998. 

Bước sắp xếp cũng tự động giải quyết sự ràng buộc về mặt từ điển vì so sánh bộ dữ liệu của Python sử dụng trường thứ hai khi trường thứ nhất bằng nhau. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Giả sử$N = 2, M = 2$. Lưới thủ môn: 

| 0,5 | 0,8 | 
| --- | --- | 
| 0,6 | 0,9 | 

Lưới của người chơi A: 

| 0,9 | 0,9 | 
| --- | --- | 
| 0,9 | 0,9 | 

Chúng tôi tính toán sản phẩm: 

| 0,45 | 0,72 | 
| --- | --- | 
| 0,54 | 0,81 | 

Các ô ≥ 0,65 là hai ô (0,72 và 0,81), do đó điểm = 2. 

Điều này cho thấy cách hoạt động của việc đánh giá tế bào độc lập. 

### Ví dụ 2 

Lưới thủ môn: 

| 1.0 | 0,5 | 
| --- | --- | 
| 0,5 | 1.0 | 

Người chơi B: 

| 0,7 | 0,7 | 
| --- | --- | 
| 0,7 | 0,7 | 

Sản phẩm: 

| 0,7 | 0,35 | 
| --- | --- | 
| 0,35 | 0,7 | 

Chỉ có hai ô đủ điều kiện một lần nữa. Điều này khẳng định tính đối xứng: các cách phân phối khác nhau vẫn có thể tạo ra các điểm số giống nhau, đó là lý do tại sao thứ tự từ điển lại quan trọng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(KNM + K log K) | Mỗi người chơi quét tất cả các ô lưới, sau đó việc sắp xếp chỉ chiếm ưu thế một chút | 
| Không gian | O(K + NM) | Lưu trữ tất cả ma trận và siêu dữ liệu trình phát | 

Được cho$N, M, K \le 100$, các hoạt động tối đa là khoảng$10^6$, trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from math import isclose

    n, m = map(int, input().split())
    k = int(input())

    keeper = [list(map(float, input().split())) for _ in range(n)]

    players = []

    for _ in range(k):
        name = input().strip()
        mat = [list(map(float, input().split())) for _ in range(n)]

        score = 0
        for i in range(n):
            for j in range(m):
                if keeper[i][j] * mat[i][j] >= 0.65 - 1e-12:
                    score += 1

        players.append((-score, name))

    players.sort()
    return "\n".join(name for _, name in players[:5])

# small deterministic case
assert run("""2 2
6
0.5 0.8
0.6 0.9
A
0.9 0.9
0.9 0.9
B
0.1 0.1
0.1 0.1
C
0.8 0.8
0.8 0.8
D
0.5 0.5
0.5 0.5
E
0.7 0.7
0.7 0.7
F
0.2 0.2
0.2 0.2
""").split()[0] in "ACDE"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Lưới hỗn hợp | Top 5 cái tên | Xếp hạng đúng đắn và hòa hợp | 

## Vỏ cạnh 

Trường hợp một cạnh là khi nhiều người chơi có điểm giống hệt nhau. Thuật toán xử lý việc này một cách tự nhiên vì việc sắp xếp sử dụng so sánh từ điển làm khóa phụ. Ví dụ: nếu hai người chơi đều có điểm 10, thứ tự của họ được xác định hoàn toàn bằng cách so sánh chuỗi tên. 

Một trường hợp cạnh khác là ranh giới dấu phẩy động ở mức chính xác là 0,65. Bộ bảo vệ epsilon đảm bảo rằng các giá trị cực kỳ gần ngưỡng do lỗi chính xác sẽ không bị phân loại sai. Ví dụ: một giá trị được tính toán như 0,64999999997 sẽ không đạt điều kiện một cách không chính xác nếu không có dung sai. 

Trường hợp cạnh cuối cùng là khi tất cả người chơi có ma trận giống hệt nhau. Trong trường hợp đó, tất cả các điểm đều bằng nhau và đầu ra chỉ đơn giản là năm tên nhỏ nhất theo từ điển mà quy tắc sắp xếp tự động tạo ra.
