---
title: "CF 1811A - Chèn chữ số"
description: "Chúng ta được cấp một số thập phân được viết dưới dạng một chuỗi và có thêm một chữ số. Chúng ta được phép chèn chữ số này vào bất kỳ vị trí nào trong số, kể cả trước chữ số đầu tiên hoặc sau chữ số cuối cùng."
date: "2026-06-09T08:37:49+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math", "strings"]
categories: ["algorithms"]
codeforces_contest: 1811
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 863 (Div. 3)"
rating: 800
weight: 1811
solve_time_s: 91
verified: true
draft: false
---

[CF 1811A - Chèn chữ số](https://codeforces.com/problemset/problem/1811/A) 

**Đánh giá:** 800 
**Tags:** tham lam, toán học, chuỗi 
**Thời gian giải:** 1 phút 31s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một số thập phân được viết dưới dạng một chuỗi và có thêm một chữ số. Chúng ta được phép chèn chữ số này vào bất kỳ vị trí nào trong số, kể cả trước chữ số đầu tiên hoặc sau chữ số cuối cùng. Mục tiêu là chọn vị trí chèn sao cho số kết quả, được hiểu bình thường trong cơ số 10 mà không có số 0 đứng đầu được viết hoa đặc biệt, càng lớn càng tốt. 

Mô hình tinh thần quan trọng là chúng ta đang chèn một ký tự vào một chuỗi và muốn có chuỗi kết quả lớn nhất có thể về mặt từ điển, vì việc so sánh hai chuỗi thập phân có độ dài bằng nhau, từng chữ số, khớp với so sánh số cho các số nguyên không âm. 

Kích thước đầu vào lớn trong các trường hợp thử nghiệm, với tổng độ dài của tất cả các số tối đa là 200.000. Điều đó ngay lập tức gợi ý rằng bất kỳ giải pháp nào thử từng vị trí chèn một cách độc lập và xây dựng lại chuỗi đầy đủ nhiều lần sẽ vẫn chỉ ổn nếu tổng hợp nó là tuyến tính cho mỗi trường hợp thử nghiệm. Bất cứ điều gì bậc hai cho mỗi trường hợp thử nghiệm sẽ sụp đổ trong trường hợp đầu vào xấu nhất. 

Một cách tiếp cận đơn giản có thể thử chèn chữ số vào mỗi vị trí trong số n+1, tạo chuỗi kết quả mỗi lần và so sánh tất cả các kết quả. Điều này sẽ tốn O(n^2) cho mỗi trường hợp thử nghiệm trong trường hợp xấu nhất do việc xây dựng và so sánh chuỗi lặp đi lặp lại. Với tổng n lên tới 2e5 qua các thử nghiệm, điều này quá chậm. 

Có một số trường hợp có thể phá vỡ trực giác tham lam bất cẩn của bạn. Một là khi chữ số phụ nhỏ, chẳng hạn như chèn 1 vào số giảm dần như 98765. Việc thêm luôn sẽ sai vì chèn sớm sẽ mang lại số lớn hơn. Một trường hợp khác là khi các chữ số bằng với chữ số được chèn, ví dụ: chèn 5 vào 5555, trong đó vị trí không quan trọng nhưng các quy tắc chèn lệch nhau có thể dễ dàng thay đổi độ chính xác nếu điều kiện nghiêm ngặt theo hướng sai. Trường hợp cạnh cuối cùng là chèn số 0, trong đó vị trí vẫn quan trọng vì các số 0 đứng đầu không được phép trong bản gốc nhưng được phép như một phần của cấu trúc trung gian; tính đúng đắn hoàn toàn phụ thuộc vào việc tối đa hóa trật tự từ điển. 

## Phương pháp tiếp cận 

Phương pháp brute-force thử mọi điểm chèn có thể. Đối với mỗi vị trí i từ 0 đến n, chúng ta xây dựng một chuỗi mới bằng cách chèn chữ số và so sánh nó với chuỗi tốt nhất được thấy cho đến nay. Điều này đúng vì nó đánh giá rõ ràng tất cả các cấu hình hợp lệ. Điểm thất bại là hiệu suất. Mỗi lần chèn tốn O(n) để xây dựng và O(n) để so sánh, đưa ra O(n^2) cho mỗi trường hợp thử nghiệm. Tổng cộng có hơn 2e5 ký tự, điều này trở nên quá chậm. 

Cấu trúc của vấn đề cho phép quan sát đơn giản hơn. Chúng tôi đang chèn một chữ số vào một chuỗi cố định và chúng tôi muốn chuỗi kết quả càng lớn càng tốt theo thứ tự từ điển. Điều này có nghĩa là chúng tôi muốn chữ số được chèn ở xa nhất có thể trong khi vẫn duy trì thứ tự tối đa. Nếu chúng ta di chuyển từ trái sang phải qua số, chúng ta nên giữ các chữ số hiện có miễn là chúng lớn hơn hoặc bằng chữ số được chèn, vì việc đặt chữ số sau chúng sẽ giữ nguyên tiền tố lớn hơn. Vị trí đầu tiên mà chữ số hiện tại hoàn toàn nhỏ hơn chữ số được chèn là nơi chúng ta nên chèn nó. 

Nếu chúng ta không bao giờ tìm được vị trí như vậy thì có nghĩa là tất cả các chữ số đều lớn hơn hoặc bằng chữ số được chèn vào, vì vậy vị trí tốt nhất là ở cuối. 

Điều này làm giảm vấn đề xuống còn một lần quét tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập.

1. Đọc số xâu và chữ số d đã chuyển thành ký tự. Làm việc với các ký tự sẽ tránh được các chuyển đổi số nguyên lặp lại và căn chỉnh trực tiếp với so sánh từ điển. 
2. Khởi tạo danh sách kết quả trống và cờ cho biết chúng tôi đã chèn chữ số hay chưa. 
3. Quét số gốc từ trái sang phải. Tại mỗi vị trí, so sánh chữ số hiện tại với d. 
4. Trong khi chữ số hiện tại lớn hơn hoặc bằng d, chúng ta sẽ thêm nó vào kết quả. Chúng tôi làm điều này vì việc đặt d sớm hơn sẽ làm cho tiền tố nhỏ hơn theo thứ tự từ điển. 
5. Ngay khi tìm thấy một chữ số hoàn toàn nhỏ hơn d, chúng ta chèn d vào vị trí này, nối nó vào kết quả, rồi nối phần còn lại của số ban đầu. 
6. Nếu đến cuối mà không chèn d, chúng ta sẽ thêm nó vào cuối. 

Điểm quyết định quan trọng là sự bất bình đẳng nghiêm ngặt. Nếu chữ số hiện tại bằng d, chúng ta vẫn trì hoãn việc chèn, vì việc đặt d sau đó sẽ giữ nguyên tiền tố và không làm giảm số kết quả. 

Tại sao nó hoạt động: tại bất kỳ thời điểm nào trong quá trình quét, tất cả các chữ số đã được thêm vào kết quả đều được đảm bảo ở mức tối ưu cho vị trí của chúng. Vị trí đầu tiên mà việc đặt d sẽ cải thiện tiền tố chính xác là nơi chữ số hiện có nhỏ hơn d. Bất kỳ lần chèn nào sau này sẽ làm giảm tiền tố sớm hơn mức cần thiết và bất kỳ lần chèn nào trước đó sẽ thay thế một chữ số lớn hơn hoặc bằng nhau, điều này không thể làm tăng giá trị từ điển cuối cùng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        n, d = input().split()
        s = input().strip()

        inserted = False
        res = []

        for ch in s:
            if not inserted and ch < d:
                res.append(d)
                inserted = True
            res.append(ch)

        if not inserted:
            res.append(d)

        out.append("".join(res))

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc triển khai trực tiếp theo sau quá trình quét tham lam được mô tả trước đó. Kết quả được xây dựng bằng cách sử dụng danh sách để đạt hiệu quả vì việc nối chuỗi lặp lại sẽ làm giảm hiệu suất đối với hành vi bậc hai. Việc kiểm tra chèn diễn ra một lần cho mỗi ký tự, đảm bảo độ phức tạp tuyến tính. 

Một điểm tinh tế là việc chèn xảy ra trước khi thêm ký tự hiện tại khi chúng tôi phát hiện ch < d. Điều này đảm bảo tính chính xác vì chữ số phải xuất hiện đúng trước chữ số nhỏ hơn đầu tiên. Thay vào đó, nếu chúng ta nối thêm trước rồi chèn vào, chúng ta sẽ dịch chuyển vị trí một cách không chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 4
76543
```Chúng tôi quét từng chữ số và theo dõi xem việc chèn có xảy ra hay không. 

| Bước | Chữ số hiện tại | d | Hành động | Kết quả cho đến nay | 
| --- | --- | --- | --- | --- | 
| 1 | 7 | 4 | 7 ≥ 4, giữ | 7 | 
| 2 | 6 | 4 | 6 ≥ 4, giữ | 76 | 
| 3 | 5 | 4 | 5 ≥ 4, giữ | 765 | 
| 4 | 4 | 4 | 4 ≥ 4, giữ | 7654 | 
| 5 | 3 | 4 | 3 < 4, chèn rồi nối thêm | 76543 + 4 được chèn trước 3 | 

Kết quả cuối cùng: 765443 

Điều này cho thấy chữ số được chèn ngay trước chữ số nhỏ hơn đầu tiên. 

### Ví dụ 2 

đầu vào:```
5 8
97531
```| Bước | Chữ số hiện tại | d | Hành động | Kết quả cho đến nay | 
| --- | --- | --- | --- | --- | 
| 1 | 9 | 8 | 9 ≥ 8 | 9 | 
| 2 | 7 | 8 | 7 < 8 chèn | 98 | 
| còn lại | 5 3 1 | 8 | nối thêm phần còn lại | 98531 | 

Kết quả cuối cùng: 98531 

Ở đây, chữ số được chèn sẽ di chuyển về phía trước càng sớm càng tốt để tối đa hóa giá trị từ điển. 

Hai dấu vết này chứng tỏ sự bất biến rằng chúng tôi luôn duy trì tiền tố tốt nhất có thể và chỉ quyết định chèn vào điểm đầu tiên nơi chữ số mới cải thiện thứ tự. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | quét tuyến tính đơn với công việc liên tục trên mỗi ký tự | 
| Không gian | O(n) | bộ đệm đầu ra lưu trữ chuỗi được xây dựng | 

Tổng kích thước đầu vào trong các trường hợp thử nghiệm được giới hạn bởi 2e5, do đó thời gian chạy tổng thể là tuyến tính theo kích thước của đầu vào và dễ dàng phù hợp với các ràng buộc. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []

    for _ in range(t):
        n, d = input().split()
        s = input().strip()

        inserted = False
        res = []

        for ch in s:
            if not inserted and ch < d:
                res.append(d)
                inserted = True
            res.append(ch)

        if not inserted:
            res.append(d)

        out.append("".join(res))

    return "\n".join(out)

# provided samples
assert run("11\n5 4\n76543\n1 0\n1\n2 5\n44\n3 6\n666\n5 6\n13579\n5 8\n97531\n19 4\n9876543210123456789\n5 7\n73737\n8 1\n20000000\n7 0\n7058959\n12 1\n828127127732\n") == \
"765443\n10\n544\n6666\n613579\n987531\n98765443210123456789\n773737\n210000000\n70589590\n8281271277321"

# custom cases
assert run("1\n3 5\n123\n") == "5123", "insert at front"
assert run("1\n3 5\n999\n") == "9995", "append at end"
assert run("1\n5 4\n44444\n") == "444444", "all equal digits"
assert run("1\n5 9\n12345\n") == "912345", "always insert at beginning"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 5 / 123 | 5123 | chèn vào đầu | 
| 3 5 / 999 | 9995 | chèn vào cuối | 
| 5 4 / 44444 | 444444 | xử lý bình đẳng | 
| 5 9 / 12345 | 912345 | sự thống trị nghiêm ngặt của chữ số lớn | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi tất cả các chữ số đều bằng chữ số được chèn. Đối với đầu vào`44444`với`d = 4`, quá trình quét không bao giờ tìm thấy một chữ số nào nhỏ hơn 4, do đó việc chèn sẽ diễn ra ở cuối. Thuật toán nối tất cả các chữ số trước, sau đó nối 4, tạo ra`444444`, điều này đúng vì bất kỳ thao tác chèn nào trước đó sẽ tạo ra cùng một tập hợp nhiều chữ số nhưng có thứ tự giống hệt nhau và quy tắc vẫn đặt chữ số ở vị trí hợp lệ cuối cùng một cách nhất quán. 

Một trường hợp cạnh khác là khi chữ số được chèn là 0. Ví dụ: chèn vào`7058959`, thuật toán đặt 0 ở chữ số đầu tiên nhỏ hơn 0, điều này không bao giờ xảy ra nên nó được thêm vào cuối. Điều này đúng vì việc chèn vị trí đầu sẽ chỉ làm giảm giá trị từ điển vì 0 là chữ số nhỏ nhất. 

Trường hợp cạnh cuối cùng là khi chữ số được chèn lớn nhất có thể, 9. Trong`12345`, chữ số đầu tiên 1 nhỏ hơn 9 nên ta chèn ngay vào đầu, tạo ra`912345`. Điều này xác nhận quy tắc tham lam ưu tiên chính xác việc tối đa hóa vị trí chữ số có ý nghĩa nhất.
