---
title: "CF 2036A - Quintomania"
description: "Giai điệu ở đây chỉ là một chuỗi ngắn các cao độ nguyên. Mỗi cao độ là một số nguyên từ 0 đến 127 và điều quan trọng không phải là giá trị tuyệt đối mà là sự khác biệt giữa các nốt lân cận."
date: "2026-06-08T10:19:06+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 2036
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 984 (Div. 3)"
rating: 800
weight: 2036
solve_time_s: 61
verified: true
draft: false
---

[CF 2036A - Quintomania](https://codeforces.com/problemset/problem/2036/A) 

**Đánh giá:** 800 
**Thẻ:** triển khai 
**Thời gian giải:** 1m 1s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Giai điệu ở đây chỉ là một chuỗi ngắn các cao độ nguyên. Mỗi cao độ là một số nguyên từ 0 đến 127 và điều quan trọng không phải là giá trị tuyệt đối mà là sự khác biệt giữa các nốt lân cận. 

Chúng tôi được yêu cầu kiểm tra một thuộc tính cấu trúc đơn giản: đối với mỗi cặp liền kề trong chuỗi, chênh lệch tuyệt đối về giá trị của chúng phải là 5 hoặc 7. Nếu mọi chuyển đổi trong giai điệu đều tôn trọng quy tắc này thì giai điệu được coi là hợp lệ, nếu không thì giai điệu sẽ bị từ chối. 

Mỗi trường hợp thử nghiệm là độc lập, do đó nhiệm vụ được lặp lại là kiểm tra nhiều mảng nhỏ. Kích thước của mỗi giai điệu nhỏ, tối đa 50 nốt và có tối đa 1000 trường hợp thử nghiệm, do đó, ngay cả việc kiểm tra từng cặp đơn giản cũng có thể nhanh chóng thoải mái. 

Không có biến đổi ẩn hoặc ràng buộc toàn cầu. Điều tinh tế duy nhất là hãy nhớ rằng điều kiện là chênh lệch tuyệt đối, không phải chênh lệch có dấu. Sự chuyển đổi như 10 thành 17 và 17 thành 10 đều hợp lệ vì cả hai đều có chênh lệch hiệu suất 7. 

Một lỗi điển hình trong việc triển khai đơn giản là quên giá trị tuyệt đối hoặc chỉ kiểm tra một hướng của sự khác biệt. Một vấn đề khác phát sinh khi ai đó vô tình kiểm tra sự bằng nhau của các nốt liên tiếp thay vì sự khác biệt của chúng. 

Các trường hợp cạnh chủ yếu có chiều dài tối thiểu. Nếu một giai điệu có đúng 2 nốt thì chúng ta chỉ kiểm tra một quãng. Ví dụ, đầu vào`[1, 6]`là hợp lệ vì chênh lệch là 5, trong khi`[1, 7]`không hợp lệ vì chênh lệch là 6, điều này không được phép. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu gần như giống với định nghĩa. Đối với mỗi giai điệu, lặp lại qua tất cả các cặp liền kề và xác minh xem mỗi chênh lệch là 5 hay 7. Nếu bất kỳ cặp nào vi phạm điều kiện, chúng tôi sẽ ngay lập tức từ chối giai điệu đó. 

Vì mỗi giai điệu có tối đa 50 nốt nên phương pháp này thực hiện tối đa 49 phép so sánh cho mỗi trường hợp thử nghiệm và nhiều nhất là 1000 trường hợp thử nghiệm, đưa ra trường hợp xấu nhất là khoảng 50.000 thao tác. Điều này vốn đã tầm thường nên không cần tối ưu hóa nâng cao. 

Không có thủ thuật thuật toán thực sự nào ẩn giấu ở đây. Quan sát chính chỉ đơn giản là điều kiện mang tính cục bộ: mọi ràng buộc chỉ liên quan đến hai phần tử liên tiếp, do đó toàn bộ vấn đề giảm xuống chỉ còn một lần xác thực vượt qua. Không có sự phụ thuộc giữa các cặp khác nhau ngoài các điểm cuối được chia sẻ, do đó không cần lập trình động hoặc tiền xử lý. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(t·n) | O(1) | Đã chấp nhận | 
| Tối ưu | O(t·n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng giai điệu một cách độc lập và xác thực nó trong một lần duy nhất. 

1. Đọc số lượng test case. Mỗi trường hợp thử nghiệm mô tả một giai điệu. 
2. Với mỗi giai điệu, hãy đọc số nốt và danh sách các số nguyên. 
3. Lặp lại từ nốt thứ hai đến nốt cuối cùng. 
4. Với mỗi chỉ số i, hãy tính hiệu tuyệt đối giữa a[i] và a[i−1]. Điều này ghi lại khoảng thời gian giữa các nốt liên tiếp bất kể hướng nào. 
5. Kiểm tra xem sự khác biệt này có chính xác là 5 hay chính xác là 7. Nếu không, hãy kết luận ngay rằng giai điệu đó không hợp lệ. 
6. Nếu vòng lặp kết thúc mà không tìm thấy vi phạm, giai điệu đáp ứng mọi ràng buộc và hợp lệ. 

Sự lựa chọn thiết kế quan trọng là thoát khỏi thất bại sớm. Sau khi tìm thấy một quãng không hợp lệ, bạn không cần phải tiếp tục quét phần còn lại của chuỗi vì một lần vi phạm cũng đủ để làm mất hiệu lực toàn bộ giai điệu. 

### Tại sao nó hoạt động 

Mọi yêu cầu trong bài toán đều độc lập và cục bộ đối với các cặp liền kề. Giai điệu có giá trị khi và chỉ khi tất cả các ràng buộc cục bộ được thỏa mãn. Thuật toán trực tiếp thực thi từng ràng buộc chính xác một lần. Vì không có bước nào sửa đổi trình tự hoặc phụ thuộc vào các quyết định trong tương lai nên không có cách nào để một trình tự hợp lệ bị từ chối không chính xác hoặc một trình tự không hợp lệ được chấp nhận. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))

    ok = True
    for i in range(1, n):
        diff = abs(a[i] - a[i - 1])
        if diff != 5 and diff != 7:
            ok = False
            break

    print("YES" if ok else "NO")
```Giải pháp được cấu trúc dưới dạng xác thực phát trực tuyến đơn giản. Mỗi trường hợp thử nghiệm được xử lý độc lập và chúng tôi tránh lưu trữ bất cứ thứ gì ngoài mảng hiện tại. 

Chi tiết triển khai quan trọng đang sử dụng`abs(a[i] - a[i - 1])`. Nếu không có giá trị tuyệt đối, hiệu âm sẽ đỗ hoặc trượt không chính xác tùy theo dấu. Một chi tiết quan trọng khác là sớm`break`, điều này đảm bảo chúng tôi dừng ngay khi phát hiện hành vi vi phạm. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
3
76 83 88
3
10 15 22
4
1 6 1 6
```| tôi | Cặp | Sự khác biệt | Có hiệu lực cho đến nay | 
| --- | --- | --- | --- | 
| 1 | 76,83 | 7 | CÓ | 
| 2 | 83,88 | 5 | CÓ | 

Kết quả: CÓ 

| tôi | Cặp | Sự khác biệt | Có hiệu lực cho đến nay | 
| --- | --- | --- | --- | 
| 1 | 10,15 | 5 | CÓ | 
| 2 | 15,22 | 7 | CÓ | 

Kết quả: CÓ 

| tôi | Cặp | Sự khác biệt | Có hiệu lực cho đến nay | 
| --- | --- | --- | --- | 
| 1 | 1,6 | 5 | CÓ | 
| 2 | 6,1 | 5 | CÓ | 
| 3 | 1,6 | 5 | CÓ | 

Kết quả: CÓ 

Hai ví dụ đầu tiên xác nhận các chuyển tiếp chuyển tiếp và hỗn hợp tiêu chuẩn, trong khi ví dụ thứ ba cho thấy chuyển động qua lại xen kẽ vẫn hợp lệ miễn là mỗi bước tôn trọng khoảng cách cho phép. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(∑n) | Mỗi trường hợp thử nghiệm quét giai điệu của nó một lần | 
| Không gian | O(1) | Chỉ có một vài biến ngoài lưu trữ đầu vào | 

Tổng số phần tử trên tất cả các trường hợp thử nghiệm đủ nhỏ để một lần quét tuyến tính cho mỗi trường hợp dễ dàng phù hợp với giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def solve():
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        ok = True
        for i in range(1, n):
            d = abs(a[i] - a[i - 1])
            if d != 5 and d != 7:
                ok = False
                break

        out.append("YES" if ok else "NO")

    print("\n".join(out))

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import io as _io
    buf = _io.StringIO()
    with redirect_stdout(buf):
        solve()
    return buf.getvalue().strip()

# provided samples
assert run("""8
2
114 109
2
17 10
3
76 83 88
8
38 45 38 80 85 92 99 106
5
63 58 65 58 65
8
117 124 48 53 48 43 54 49
5
95 102 107 114 121
10
72 77 82 75 70 75 68 75 68 75
""") == """YES
YES
YES
NO
YES
NO
YES
YES"""

# minimum length valid
assert run("""1
2
0 5
""") == "YES"

# minimum length invalid
assert run("""1
2
0 6
""") == "NO"

# alternating valid pattern
assert run("""1
4
0 5 0 5
""") == "YES"

# single violation in middle
assert run("""1
5
0 5 10 18 23
""") == "NO"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2→5→0→5 | CÓ | xen kẽ các bước hợp lệ | 
| 0→6 | KHÔNG | cạnh đơn không hợp lệ | 
| 0→5→10→18→23 | KHÔNG | lan truyền vi phạm giữa chuỗi | 

## Vỏ cạnh 

Trường hợp một cạnh là độ dài giai điệu nhỏ nhất có thể, n = 2. Ví dụ: đầu vào`0 5`sẽ trả về CÓ vì có chính xác một khoảng và nó khớp với quy tắc. Thuật toán xử lý việc này một cách chính xác vì vòng lặp chạy một lần và xác thực sự khác biệt duy nhất đó. 

Một trường hợp khác là một chuỗi thay đổi các hướng nhưng vẫn giữ kích thước bước hợp lệ, chẳng hạn như`0 5 0 5`. Sự khác biệt là 5, 5 và 5, tất cả đều hợp lệ, vì vậy giai điệu sẽ trôi qua. Thuật toán hỗ trợ điều này một cách tự nhiên vì nó không giả định chuyển động đơn điệu. 

Trường hợp thất bại là khi một quá trình chuyển đổi không hợp lệ xuất hiện ở giữa. Ví dụ`0 5 10 18`có chênh lệch 5, 5 và 8. Ở bước từ 10 đến 18, chênh lệch là 8, điều này ngay lập tức gây ra sự từ chối. Việc thoát sớm đảm bảo chúng tôi không tiếp tục sai và bỏ qua hành vi vi phạm.
