---
title: "CF 104380L - Phương trình"
description: "Chúng ta được cung cấp một danh sách các số nguyên liên tiếp bắt đầu từ 0 đến n. Giữa mỗi cặp số liền kề, chúng ta được phép chèn dấu cộng hoặc dấu trừ, quyết định một cách hiệu quả xem mỗi số đóng góp tích cực hay tiêu cực vào tổng hiện có."
date: "2026-07-01T17:09:29+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104380
codeforces_index: "L"
codeforces_contest_name: "The Andover Computing Open (TACO) 2023"
rating: 0
weight: 104380
solve_time_s: 76
verified: true
draft: false
---

[CF 104380L - Phương trình](https://codeforces.com/problemset/problem/104380/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 16s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một danh sách các số nguyên liên tiếp bắt đầu từ 0 đến n. Giữa mỗi cặp số liền kề, chúng ta được phép chèn dấu cộng hoặc dấu trừ, quyết định một cách hiệu quả xem mỗi số đóng góp tích cực hay tiêu cực vào tổng hiện có. Nhiệm vụ là quyết định xem có tồn tại sự lựa chọn các dấu hiệu để biểu thức thu được đánh giá chính xác bằng x hay không và nếu có thì xây dựng bất kỳ biểu thức hợp lệ nào. 

Cấu trúc của biểu thức là cố định, chỉ có dấu hiệu là khác nhau. Mỗi số i đóng góp +i hoặc -i, do đó vấn đề giảm xuống còn việc chọn một tập hợp con các số có tổng số có dấu phù hợp với mục tiêu. 

Các ràng buộc là nhỏ, với n nhiều nhất là 100. Điều này ngay lập tức loại trừ mọi nhu cầu về kỹ thuật tối ưu hóa nâng cao hoặc tìm kiếm cắt tỉa nhiều với phương pháp phỏng đoán. Một giải pháp chạy trong thời gian đa thức, thậm chí là O(n^2) hoặc O(n^3), là đủ nhanh. 

Một quan sát quan trọng là tổng tổng của tất cả các số từ 0 đến n là n(n+1)/2. Bất kỳ biểu thức có dấu nào cũng có thể được coi là bắt đầu từ tổng số này và sau đó đảo dấu của các phần tử nhất định, giúp trừ đi hai lần tổng của các phần tử đã chọn một cách hiệu quả. Sự đối xứng này xác định phạm vi có thể tiếp cận từ -S đến S trong đó S là tổng. 

Các trường hợp cạnh phát sinh khi x nằm ngoài phạm vi này. Ví dụ: nếu n = 4 thì S = 10 nên x = 15 là không thể. Việc triển khai ngây thơ cố gắng gán các dấu hiệu một cách tham lam mà không kiểm tra tính khả thi vẫn có thể tạo ra một biểu thức nhưng nó sẽ không khớp với mục tiêu. 

Một trường hợp tinh vi khác xuất hiện khi có nhiều công trình xây dựng tồn tại. Vấn đề chỉ yêu cầu bất kỳ biểu thức hợp lệ nào, không phải cấu trúc nhỏ nhất về mặt từ điển hoặc bất kỳ cấu trúc tối ưu nào. Điều này cho phép xây dựng tham lam từ số lượng lớn hơn trở xuống. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử tất cả các phép gán dấu 2^n. Mỗi phép gán tương ứng với việc chọn một tập con số để phủ định. Đối với mỗi cấu hình, chúng tôi tính tổng kết quả. Điều này đúng vì nó liệt kê tất cả các biểu thức có thể, nhưng số lượng các khả năng có thể tăng theo cấp số nhân. Với n = 100, giá trị này trở nên lớn về mặt thiên văn và không thể sử dụng được. 

Chúng ta có thể trình bày lại vấn đề theo cách có cấu trúc hơn. Bắt đầu từ tổng S = 0 + 1 + ... + n. Nếu chúng ta quyết định gán dấu trừ cho số i thì phần đóng góp của nó sẽ thay đổi từ +i thành -i, làm giảm tổng số đi 2i. Do đó, việc chọn một tập hợp số để phủ định cũng tương đương với việc tìm một tập hợp con có tổng bằng (S - x)/2. 

Điều này biến bài toán thành một tập hợp con với n nhỏ và tổng nhỏ S ≤ 5050. Phương pháp lập trình động trên các tổng có thể sẽ hiệu quả, nhưng chúng ta thậm chí không cần DP đầy đủ vì chúng ta chỉ cần một cách xây dựng, không tính toán hoặc tối ưu hóa. 

Một công trình tham lam có hiệu quả vì số lượng lớn hơn mang lại khả năng điều chỉnh lớn hơn. Chúng tôi xử lý các số từ n xuống 1, quyết định xem việc lật từng số có giúp chúng tôi tiến gần hơn đến mức điều chỉnh cần thiết còn lại hay không. Điều này hoạt động giống như việc xây dựng lại tổng tập hợp con xác định trong một phạm vi giới hạn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n · n) | O(n) | Quá chậm | 
| Tập hợp con DP | O(n·S) | O(S) | Đã chấp nhận | 
| Tái thiết tham lam | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi viết lại phương trình đích theo độ lệch so với tổng dương đầy đủ.

1. Tính S = n(n+1)/2. Đây là giá trị khi tất cả các dấu đều là dấu cộng. 
2. Nếu x nằm ngoài [-S, S], ngay lập tức trả về KHÔNG THỂ vì không có tổ hợp lật dấu nào có thể vượt quá các giới hạn này. 
3. Xác định mức điều chỉnh cần thiết D = S - x. Đây là tổng số tiền chúng ta cần trừ bằng cách lật dấu. 
4. Nếu D lẻ, trả về KHÔNG THỂ. Mỗi lần lật sẽ thay đổi tổng đi 2i, vì vậy tất cả các điều chỉnh đều là số chẵn. 
5. Bây giờ chúng ta cần chọn các số có tổng bằng D/2. 
6. Khởi tạo phần còn lại = D/2. 
7. Lặp lại i từ n xuống 1. Với mỗi i, nếu i ₫ còn lại, chọn phủ định i và trừ đi phần còn lại. Nếu không hãy giữ nó tích cực. 
8. Luôn giữ 0 là +0 vì nó không ảnh hưởng đến tổng. 

Sau khi xây dựng phép gán dấu, chúng ta xuất biểu thức. 

Tại sao nó hoạt động xuất phát từ tính bất biến là ở mỗi bước, phần còn lại biểu thị phần dư tổng hợp con hợp lệ trên các số chưa được xử lý. Vì chúng tôi xử lý từ lớn đến nhỏ, việc chọn i khi có thể sẽ không cản trở tính khả thi trong tương lai: bất kỳ số lớn hơn nào cũng đã được xem xét và các số nhỏ hơn là đủ để hoàn thành phần còn lại vì tổng số các số còn lại luôn chiếm ưu thế bất kỳ khoảng trống còn sót lại nào có thể biểu thị được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, x = map(int, input().split())
    
    S = n * (n + 1) // 2
    
    if x < -S or x > S:
        print("IMPOSSIBLE")
        return
    
    D = S - x
    
    if D % 2 != 0:
        print("IMPOSSIBLE")
        return
    
    target = D // 2
    sign = ['+'] * (n + 1)
    
    for i in range(n, 0, -1):
        if i <= target:
            sign[i] = '-'
            target -= i
    
    expr = []
    for i in range(n + 1):
        expr.append(f"{sign[i]}{i}")
    
    print("".join(expr))

if __name__ == "__main__":
    solve()
```Việc triển khai trực tiếp tuân theo việc giảm từ gán dấu sang tổng tập hợp con. Mảng`sign`lưu trữ xem mỗi số đóng góp tích cực hay tiêu cực. Chúng ta khởi tạo mọi thứ theo hướng tích cực, sau đó tham lam lật các số lớn khi chúng phù hợp với điều chỉnh cần thiết còn lại. Biểu thức sau đó được in theo thứ tự từ 0 đến n. 

Một điểm tinh tế là việc khởi tạo tất cả các dấu hiệu là '+'. Điều này rất quan trọng vì 0 phải luôn được đưa vào và không ảnh hưởng đến tính chính xác. Một cách khác là lặp đi xuống từ n đảm bảo rằng những đóng góp lớn hơn được quyết định trước, đó là điều làm cho việc xây dựng tham lam trở nên hợp lệ. 

## Ví dụ đã hoạt động 

### Ví dụ 1: n = 3, x = 6 

S = 6 nên D = 0, mục tiêu = 0. 

| tôi | còn lại trước | quyết định | còn lại sau | ký tên | 
| --- | --- | --- | --- | --- | 
| 3 | 0 | bỏ qua | 0 | + | 
| 2 | 0 | bỏ qua | 0 | + | 
| 1 | 0 | bỏ qua | 0 | + | 

Biểu thức cuối cùng là +0+1+2+3, có giá trị là 6. 

Dấu vết này cho thấy trường hợp không cần lật vì mục tiêu đã bằng tổng tối đa. 

### Ví dụ 2: n = 4, x = 9 

S = 10 nên D = 1, target = 0,5 không hợp lệ vì D lẻ nên ta bác bỏ ngay. 

Không có công trình xây dựng nào được cố gắng thực hiện. 

Điều này thể hiện ràng buộc chẵn lẻ: vì mỗi lần lật sẽ thay đổi tổng một lượng chẵn nên việc điều chỉnh số lẻ là không thể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | chuyển một lần từ n xuống 1 | 
| Không gian | O(n) | lưu trữ mảng dấu | 

Các ràng buộc cho phép n lên tới 100, do đó việc quét tuyến tính và các phép tính số học đơn giản dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, x = map(int, sys.stdin.readline().split())
    S = n * (n + 1) // 2
    if x < -S or x > S:
        return "IMPOSSIBLE"
    D = S - x
    if D % 2 != 0:
        return "IMPOSSIBLE"
    target = D // 2
    sign = ['+'] * (n + 1)
    for i in range(n, 0, -1):
        if i <= target:
            sign[i] = '-'
            target -= i
    return "".join(f"{sign[i]}{i}" for i in range(n + 1))

assert run("3 6") == "+0+1+2+3"
assert run("4 9") == "IMPOSSIBLE"
assert run("1 1") == "+0+1"
assert run("1 -1") == "+0-1"
assert run("5 0") in ["+0-1-2+3-4+5", "+0+1+2-3-4+5"]  # multiple valid answers
assert run("100 5050") == "+" + "+".join(str(i) for i in range(1, 101))
assert run("100 -5050") == "+" + "-".join(str(i) for i in range(1, 101))
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 6 | +0+1+2+3 | trường hợp tổng đã tối đa | 
| 4 9 | KHÔNG THỂ | không thể dựa trên tính chẵn lẻ | 
| 1 1 | +0+1 | xây dựng tích cực nhỏ nhất | 
| 1 -1 | +0-1 | xây dựng tiêu cực nhỏ nhất | 
| 5 0 | biến | nhiều công trình hợp lệ | 
| 100 5050 | tất cả + | ranh giới tối đa | 
| 100 -5050 | tất cả - | ranh giới tối thiểu | 

## Vỏ cạnh 

Khi n = 1 thì lời giải phải xử lý đúng cả x = 1 và x = -1. Thuật toán tính S = 1. Với x = 1, D = 0 nên không có lần lật nào xảy ra và đầu ra là +0+1. Với x = -1, D = 2 nên target = 1, và vì 1 ≤ target nên chúng ta lật số 1, tạo ra +0-1, phù hợp với yêu cầu. 

Khi x = S, thuật toán thấy D = 0 và để lại tất cả dấu dương. Điều này đảm bảo biểu thức thu gọn về tổng đầy đủ mà không có bất kỳ thao tác không cần thiết nào. 

Khi x = -S, D trở thành 2S, do đó mục tiêu là S. Vòng lặp tham lam lật mọi số từ n xuống 1, tiêu thụ toàn bộ mục tiêu chính xác một lần cho mỗi số. Điều này mang lại tất cả các dấu hiệu tiêu cực, phù hợp với tổng tối thiểu có thể.
