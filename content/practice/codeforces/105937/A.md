---
title: "CF 105937A - Bậc thầy thẻ"
description: "Chúng ta được tham gia một vòng chơi bài rất đơn giản, trong đó có đúng ba lá bài được rút. Mỗi lá bài có giá trị nguyên từ 1 đến 13. Điểm của vòng đấu thường chỉ là tổng của ba giá trị."
date: "2026-06-22T15:45:58+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105937
codeforces_index: "A"
codeforces_contest_name: "2025 Xian Jiaotong University Programming Contest"
rating: 0
weight: 105937
solve_time_s: 54
verified: true
draft: false
---

[CF 105937A - Bậc thầy thẻ](https://codeforces.com/problemset/problem/105937/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 54s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được tham gia một vòng chơi bài rất đơn giản, trong đó có đúng ba lá bài được rút. Mỗi lá bài có giá trị nguyên từ 1 đến 13. Điểm của vòng đấu thường chỉ là tổng của ba giá trị. Tuy nhiên, có một quy tắc đặc biệt: nếu cả ba lá bài đều có cùng giá trị, người chơi có điều kiện “trận ba” và điểm cuối cùng được tăng thêm 100 điểm. 

Vì vậy, nhiệm vụ hoàn toàn là tính tổng sửa đổi của ba số nguyên với phần thưởng có điều kiện dựa trên việc liệu cả ba đầu vào có giống nhau hay không. 

Các ràng buộc rất nhỏ vì chúng tôi chỉ đọc ba số trong một trường hợp thử nghiệm. Điều này ngay lập tức loại trừ mọi nhu cầu về cấu trúc dữ liệu hoặc mối quan tâm tối ưu hóa. Ngay cả việc kiểm tra có điều kiện đơn giản sau khi tính tổng cũng đủ. 

Không có trường hợp ẩn nào liên quan đến việc đặt hàng hoặc nhiều trường hợp thử nghiệm. Góc có ý nghĩa duy nhất là khi tất cả các giá trị đều bằng nhau, kể cả trường hợp nhỏ nhất có thể xảy ra như 1 1 1 hoặc lớn nhất như 13 13 13. Một ranh giới tầm thường khác là khi hai giá trị khớp nhau nhưng giá trị thứ ba khác nhau, ví dụ: 5 5 6, trong đó không nên áp dụng phần thưởng mặc dù có sự lặp lại một phần. 

Một sai lầm ngây thơ ở đây là áp dụng không chính xác phần thưởng bất cứ khi nào hai lá bài bất kỳ khớp nhau, thay vì yêu cầu cả ba lá bài phải khớp chính xác. 

## Phương pháp tiếp cận 

Quan điểm brute-force là giải thích các quy tắc theo nghĩa đen: tính tổng của cả ba lá bài, sau đó kiểm tra xem ba giá trị có bằng nhau hay không. Nếu có, hãy thêm tiền thưởng. 

Không có sự bùng nổ tổ hợp hoặc không gian tìm kiếm thực sự ở đây vì kích thước đầu vào là không đổi. Ngay cả khi chúng tôi tưởng tượng việc khái quát hóa điều này cho nhiều thẻ hơn, cấu trúc vẫn sẽ là một lần kiểm tra tính bằng nhau qua một lần trên bộ sưu tập. 

Điều quan trọng cần lưu ý là quy tắc tính điểm được chia thành hai phần độc lập: thành phần cộng (tổng) và chỉ báo có điều kiện (tất cả đều bằng nhau). Tổng luôn tuyến tính theo số lượng quân bài và điều kiện đẳng thức giảm xuống việc kiểm tra xem a1 có bằng a2 và a2 có bằng a3 hay không. 

Vì vậy, lời giải chỉ là một phép tính số học và so sánh, làm cho nó có hiệu quả là O(1). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (mô phỏng trực tiếp) | O(1) | O(1) | Đã chấp nhận | 
| Tối ưu (kiểm tra tổng + đẳng thức) | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc ba số nguyên a1, a2 và a3 từ đầu vào. Chúng đại diện cho giá trị của các lá bài được rút. 
2. Tính điểm cơ bản bằng cách tính tổng chúng: s = a1 + a2 + a3. Điều này phản ánh quy tắc tính điểm thông thường mà không có bất kỳ phần thưởng nào. 
3. Kiểm tra xem cả ba giá trị có giống nhau hay không bằng cách xác minh a1 == a2 và a2 == a3. Chuỗi so sánh hai này là đủ vì đẳng thức có tính bắc cầu. 
4. Nếu điều kiện đúng, hãy tăng điểm lên 100. Nếu không, hãy giữ nguyên. 
5. Xuất kết quả cuối cùng. 

### Tại sao nó hoạt động 

Hàm tính điểm được định nghĩa là tổng tuyến tính cộng với hàm chỉ báo nhị phân chỉ phụ thuộc vào việc cả ba đầu vào có bằng nhau hay không. Kiểm tra đẳng thức nắm bắt chính xác định nghĩa của cấu hình “quả bom” và tổng không phụ thuộc vào cấu trúc sắp xếp hoặc lặp lại. Vì không có sự tương tác nào khác giữa các thẻ nên việc kết hợp hai phép tính này sẽ tạo ra số điểm yêu cầu chính xác mà không bỏ sót hoặc tính hai lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

a1, a2, a3 = map(int, input().split())

score = a1 + a2 + a3

if a1 == a2 == a3:
    score += 100

print(score)
```Giải pháp đọc ba số nguyên trên một dòng và tính tổng của chúng một cách trực tiếp. Sự so sánh có dây chuyền`a1 == a2 == a3`là cách ngắn gọn và chính xác nhất để xác minh rằng cả ba giá trị đều giống nhau. 

Phép cộng có điều kiện 100 chỉ được kích hoạt trong trường hợp “tất cả đều bằng nhau”, phù hợp với định nghĩa về một quả bom. Không cần logic gì thêm vì không có quy tắc hoặc tương tác bổ sung. 

## Ví dụ đã hoạt động 

### Ví dụ 1:`6 6 6`| Bước | a1 | a2 | a3 | Tổng hợp | Kiểm tra bằng nhau | Điểm | 
| --- | --- | --- | --- | --- | --- | --- | 
| Ban đầu | 6 | 6 | 6 | 0 | - | 0 | 
| Sau tổng | 6 | 6 | 6 | 18 | - | 18 | 
| Kiểm tra | 6 | 6 | 6 | 18 | Đúng | 118 | 

Trường hợp này kích hoạt phần thưởng vì cả ba giá trị đều khớp chính xác. Kết quả cuối cùng xác nhận rằng quy tắc cộng 100 vào tổng bình thường. 

### Ví dụ 2:`1 1 4`| Bước | a1 | a2 | a3 | Tổng hợp | Kiểm tra bằng nhau | Điểm | 
| --- | --- | --- | --- | --- | --- | --- | 
| Ban đầu | 1 | 1 | 4 | 0 | - | 0 | 
| Sau tổng | 1 | 1 | 4 | 6 | - | 6 | 
| Kiểm tra | 1 | 1 | 4 | 6 | Sai | 6 | 

Ở đây, hai lá bài giống nhau nhưng lá thứ ba khác nhau nên tiền thưởng không được áp dụng. Điều này khẳng định rằng sự bình đẳng một phần là không đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ một số phép tính và so sánh số học cố định được thực hiện | 
| Không gian | O(1) | Không sử dụng cấu trúc dữ liệu phụ trợ | 

Các ràng buộc liên quan đến chính xác ba số nguyên, do đó, giải pháp là không đáng kể trong cả giới hạn thời gian và bộ nhớ. Ngay cả trong môi trường thử nghiệm khối lượng lớn, việc tính toán này diễn ra theo thời gian không đổi cho mỗi lần thử nghiệm. 

## Trường hợp thử nghiệm```python
import sys, io

def solve():
    a1, a2, a3 = map(int, input().split())
    score = a1 + a2 + a3
    if a1 == a2 == a3:
        score += 100
    print(score)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import io as sio
    out = sio.StringIO()
    with redirect_stdout(out):
        solve()
    sys.stdin = old_stdin
    return out.getvalue().strip()

# provided samples
assert run("6 6 6") == "118"
assert run("1 1 4") == "6"

# custom cases
assert run("1 2 3") == "6"
assert run("13 13 13") == "139"
assert run("5 5 6") == "16"
assert run("1 2 2") == "5"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 2 3 | 6 | Không bình đẳng, không có thưởng | 
| 13 13 13 | 139 | Giá trị tối đa có thưởng | 
| 5 5 6 | 16 | Trận đấu một phần không kích hoạt tiền thưởng | 
| 1 2 2 | 5 | Đặt hàng hỗn hợp vẫn không có thưởng | 

## Vỏ cạnh 

Trường hợp cạnh có ý nghĩa duy nhất là phân biệt đẳng thức hoàn toàn với đẳng thức một phần. Đối với đầu vào`5 5 6`, thuật toán tính tổng 16 và bỏ qua phần thưởng một cách chính xác vì`a1 == a2`là đúng nhưng`a2 == a3`là sai, do đó đẳng thức xâu chuỗi không thành công. 

Vì`1 1 1`, điều kiện`a1 == a2 == a3`được đánh giá là đúng, vì vậy thuật toán cộng 100 vào tổng cơ số 3, tạo ra 103. Điều này trực tiếp khớp với quy tắc “bom”. 

Vì`13 13 13`, logic tương tự được áp dụng ở ranh giới trên, tạo ra 39 + 100 = 139. Logic so sánh không bị ảnh hưởng bởi độ lớn giá trị, xác nhận tính chính xác trên toàn bộ phạm vi đầu vào.
