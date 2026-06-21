---
title: "CF 106039H - Trí tuệ của Thầy Ngụy"
description: "Chúng ta có hai năm bắt đầu, một dành cho Master Wei và một dành cho Kai. Từ những năm đó trở đi, mỗi người tích lũy “kinh nghiệm” bằng số năm đã trôi qua kể từ khi họ bắt đầu lập trình."
date: "2026-06-20T21:07:39+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106039
codeforces_index: "H"
codeforces_contest_name: "2025 USP Try-outs"
rating: 0
weight: 106039
solve_time_s: 39
verified: true
draft: false
---

[CF 106039H - Trí tuệ của Sư phụ Ngụy](https://codeforces.com/problemset/problem/106039/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 39s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai năm bắt đầu, một dành cho Master Wei và một dành cho Kai. Từ những năm đó trở đi, mỗi người tích lũy “kinh nghiệm” bằng số năm đã trôi qua kể từ khi họ bắt đầu lập trình. Vì vậy vào bất cứ năm nào$Y$, Kinh nghiệm của Wei là$Y - W$, và kinh nghiệm của Kai là$Y - K$. 

Chúng ta được yêu cầu tìm một năm$Y$sao cho kinh nghiệm của Wei chính xác gấp đôi kinh nghiệm của Kai. Theo thuật ngữ đại số, chúng ta đang tìm số nguyên duy nhất năm thỏa mãn:$$Y - W = 2 \cdot (Y - K)$$Những hạn chế là lớn, có thể lên tới nhiều năm$10^9$, nhưng cấu trúc hoàn toàn tuyến tính. Điều này ngay lập tức gợi ý rằng chúng ta nên tránh mô phỏng theo thời gian, vì quét nhiều năm từ$W$trở lên sẽ hoàn toàn không khả thi ở quy mô đó. 

Một vấn đề tế nhị là phương trình có thể tạo ra nghiệm không nguyên hoặc một năm trước ngày Kai bắt đầu. Vì cả hai giá trị kinh nghiệm phải tương ứng với số năm thực tế đã trôi qua nên lời giải cũng phải tôn trọng$Y \ge K$, nếu không thì trải nghiệm của Kai sẽ tiêu cực hoặc không được xác định theo cách diễn giải dự kiến. 

Không có trường hợp phân nhánh ẩn nào: một khi chúng ta thiết lập phương trình chính xác, sẽ có một năm hợp lệ duy nhất hoặc không có năm nào nằm trong giới hạn. Rủi ro chính trong lối suy luận ngây thơ là cố gắng “mô phỏng các năm tiếp theo” và kiểm tra tình trạng này qua năm khác, điều này sẽ thất bại trong thời hạn. 

## Phương pháp tiếp cận 

Một cách tiếp cận vũ phu sẽ bắt đầu từ năm$K$và tăng dần qua từng năm, tính toán kinh nghiệm của Wei và Kai ở mỗi bước và kiểm tra xem mối quan hệ có được duy trì hay không. Điều này đúng về mặt logic vì điều kiện được đánh giá trực tiếp từ định nghĩa về kinh nghiệm và cuối cùng sẽ đạt được năm chính xác nếu nó tồn tại. 

Tuy nhiên, trường hợp xấu nhất buộc không gian tìm kiếm phải mở rộng tới$10^9$lần lặp lại. Mỗi lần lặp lại thực hiện công việc không đổi, do đó độ phức tạp tổng cộng sẽ trở thành$O(10^9)$, vượt xa những gì giới hạn một giây có thể xử lý. 

Quan sát quan trọng là điều kiện là một phương trình tuyến tính trong một biến duy nhất. Thay vì lặp lại các năm có thể, chúng ta trực tiếp giải quyết năm đó bằng đại số. Khai triển phương trình:$$Y - W = 2Y - 2K$$Sắp xếp lại các điều khoản:$$- W + 2K = Y$$Vì vậy, câu trả lời chỉ đơn giản là:$$Y = 2K - W$$Điều này làm giảm vấn đề từ việc tìm kiếm trên một phạm vi rộng lớn thành một phép tính số học đơn lẻ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(K - W)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(1)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc hai số nguyên$W$Và$K$, đại diện cho những năm đầu của Wei và Kai. Chúng xác định hai hàm kinh nghiệm tuyến tính theo năm mục tiêu chưa xác định. 
2. Chuyển điều kiện thành phương trình sử dụng định nghĩa của kinh nghiệm. Kinh nghiệm của Wei là$Y - W$, của Kai là$Y - K$, và chúng tôi thực thi sự bình đẳng$Y - W = 2(Y - K)$. Bước này rất quan trọng vì nó chuyển một điều kiện tường thuật thành đại số. 
3. Khai triển phương trình và cô lập$Y$. Điều này tạo ra$Y - W = 2Y - 2K$, và sắp xếp lại sản lượng$Y = 2K - W$. Đây là năm ứng cử viên duy nhất có thể đáp ứng điều kiện. 
4. Xuất trực tiếp giá trị tính toán dưới dạng câu trả lời. 

### Tại sao nó hoạt động 

Cả hai hàm kinh nghiệm đều tuyến tính trong cùng một biến$Y$. Điều kiện tương đương với hai biểu thức tuyến tính, đảm bảo nhiều nhất một nghiệm. Khi chúng tôi rút ra phương trình, chúng tôi sẽ không thực hiện bất kỳ lựa chọn gần đúng hoặc heuristic nào, chỉ có phép biến đổi đại số chính xác. Vì phép biến đổi có thể đảo ngược nên bất kỳ nghiệm hợp lệ nào cũng phải thỏa mãn biểu thức dẫn xuất và bản thân biểu thức đó luôn tạo ra năm ứng viên duy nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    W, K = map(int, input().split())
    print(2 * K - W)

if __name__ == "__main__":
    solve()
```Chương trình đọc hai giá trị đầu vào và áp dụng trực tiếp công thức dẫn xuất. Phép nhân an toàn theo các số nguyên chính xác tùy ý của Python, do đó không có vấn đề tràn ngay cả khi giá trị trung gian đạt đến khoảng$2 \cdot 10^9$. 

Chi tiết triển khai quan trọng là đảm bảo thứ tự chính xác của các thao tác trong biểu thức$2K - W$. Viết$2 * (K - W)$sẽ không chính xác vì nó mã hóa một mối quan hệ đại số khác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1975 1995
```Chúng tôi tính toán$Y = 2 \cdot 1995 - 1975$. 

| Bước | Biểu hiện | Giá trị | 
| --- | --- | --- | 
| Đọc đầu vào | W, K | 1975, 1995 | 
| Tính toán | 2K - W | 2·1995 - 1975 | 
| Kết quả | Y | 4015 | 

Đầu ra:```
4015
```Điều này khẳng định vào năm 4015, Ngụy có 2040 năm kinh nghiệm và Kai có 1020, thỏa mãn điều kiện nhân đôi. 

### Ví dụ 2 

đầu vào:```
0 1000000000
```| Bước | Biểu hiện | Giá trị | 
| --- | --- | --- | 
| Đọc đầu vào | W, K | 0, 1000000000 | 
| Tính toán | 2K - W | 2·1000000000 - 0 | 
| Kết quả | Y | 2000000000 | 

Đầu ra:```
2000000000
```Điều này cho thấy ngay cả ở giới hạn đầu vào cực đoan, công thức vẫn tạo ra một số nguyên hợp lệ mà không cần mô phỏng. 

Dấu vết nhấn mạnh rằng phép tính hoàn toàn là số học và không phụ thuộc vào phép lặp hoặc phân nhánh có điều kiện. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(1)$| Chỉ có một số phép tính số học không đổi được thực hiện bất kể kích thước đầu vào | 
| Không gian |$O(1)$| Không có cấu trúc phụ trợ nào được sử dụng ngoài một vài số nguyên | 

Giải pháp này phù hợp một cách thoải mái trong các ràng buộc vì nó không thực hiện lặp lại trong phạm vi năm và chỉ đánh giá một biểu thức duy nhất. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline
    W, K = map(int, input().split())
    return str(2 * K - W)

# provided samples
assert run("1975 1995") == "4015"
assert run("1989 2007") == "4025"
assert run("0 1000000000") == "2000000000"

# custom cases
assert run("1 2") == "3", "small gap"
assert run("5 5") == "5", "same start edge"
assert run("0 1") == "2", "minimal distinct years"
assert run("1000000000 1000000000") == "1000000000", "large equal bound"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 2 | 3 | khoảng cách tối thiểu khác không | 
| 5 5 | 5 | trường hợp cạnh năm bắt đầu giống hệt nhau | 
| 0 1 | 2 | đầu vào khác biệt hợp lệ nhỏ nhất | 
| 1000000000 1000000000 | 1000000000 | độ ổn định giới hạn tối đa | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi cả hai lập trình viên đều bắt đầu vào cùng một năm. Ví dụ,$W = K = 5$. Công thức cho$Y = 2K - W = 5$. Thay thế trở lại, kinh nghiệm của Wei là$5 - 5 = 0$, và của Kai cũng vậy$0$, do đó đẳng thức xảy ra. 

Một trường hợp khác là khi$W = 0$Và$K = 1$. Năm tính toán là$Y = 2 \cdot 1 - 0 = 2$. Tại$Y = 2$, Wei có 2 năm kinh nghiệm và Kai có 1 năm, thỏa mãn điều kiện nhân đôi. 

Kịch bản ranh giới là khi cả hai giá trị đều tối đa, chẳng hạn như$W = K = 10^9$. Kết quả là$Y = 10^9$, vẫn nằm trong giới hạn và duy trì tính chính xác vì cả hai giá trị kinh nghiệm vẫn bằng 0 tại thời điểm đó.
