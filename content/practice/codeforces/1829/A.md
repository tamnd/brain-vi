---
title: "CF 1829A - Chuyện Tình"
description: "Chúng ta được cung cấp một chuỗi tham chiếu cố định, “codeforces”, và đối với mỗi trường hợp thử nghiệm, chúng ta nhận được một chuỗi khác có cùng độ dài, chính xác là 10 chữ cái viết thường. Nhiệm vụ là so sánh hai chuỗi này theo vị trí và đếm xem có bao nhiêu vị trí chứa các ký tự khác nhau."
date: "2026-06-09T07:16:40+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "strings"]
categories: ["algorithms"]
codeforces_contest: 1829
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 871 (Div. 4)"
rating: 800
weight: 1829
solve_time_s: 60
verified: true
draft: false
---

[CF 1829A - Chuyện tình](https://codeforces.com/problemset/problem/1829/A) 

**Đánh giá:** 800 
**Tags:** triển khai, chuỗi 
**Thời gian giải:** 1 phút 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi tham chiếu cố định, “codeforces”, và đối với mỗi trường hợp thử nghiệm, chúng ta nhận được một chuỗi khác có cùng độ dài, chính xác là 10 chữ cái viết thường. Nhiệm vụ là so sánh hai chuỗi này theo vị trí và đếm xem có bao nhiêu vị trí chứa các ký tự khác nhau. 

Nói cách khác, hãy tưởng tượng xếp hai hàng gồm 10 ký tự: từ tham chiếu ở trên cùng và chuỗi đã cho ở bên dưới nó. Chúng tôi quét từ trái sang phải và đếm những điểm không khớp. Mỗi điểm không khớp đóng góp 1 vào câu trả lời cuối cùng và các vị trí trùng khớp đóng góp 0. Kết quả đầu ra cho mỗi trường hợp kiểm thử chỉ là số lượng không khớp này. 

Các ràng buộc đủ nhỏ để mỗi ca kiểm thử có thể được xử lý độc lập trong thời gian không đổi. Với tối đa 1000 chuỗi và mỗi chuỗi có độ dài cố định ở mức 10, ngay cả việc so sánh trực tiếp từng ký tự cũng không đáng kể trong giới hạn thời gian. Điều này ngay lập tức loại trừ mọi nhu cầu về kỹ thuật tiền xử lý, băm hoặc chuỗi nâng cao. 

Không có trường hợp cạnh tinh vi nào về mặt cấu trúc vì độ dài được cố định và đẳng thức được xác định rõ ràng theo từng ký tự. Lỗi tiềm ẩn duy nhất đến từ việc lập chỉ mục riêng lẻ hoặc vô tình so sánh với chuỗi tham chiếu đã sửa đổi. 

Một cách tiếp cận không chính xác điển hình là đếm các kết quả trùng khớp thay vì không khớp và quên chuyển đổi hoặc so sánh các phiên bản chuỗi được sắp xếp, làm mất thông tin vị trí. Ví dụ: so sánh “forcescode” với “codeforces” sau khi sắp xếp sẽ gợi ý không chính xác sự tương đồng hoàn toàn, nhưng vấn đề hoàn toàn phụ thuộc vào vị trí. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực chính xác là những gì vấn đề gợi ý: đối với mỗi trường hợp thử nghiệm, hãy so sánh từng vị trí trong số 10 vị trí với ký tự tương ứng trong “codeforces” và tăng bộ đếm bất cứ khi nào chúng khác nhau. Vì mỗi chuỗi có độ dài không đổi nên chuỗi này chạy trong thời gian không đổi cho mỗi trường hợp thử nghiệm. 

Tổng tác phẩm tối đa là so sánh 1000 × 10 ký tự, không đáng kể. Không có nút cổ chai tiệm cận nào cần cải thiện, nhưng việc đơn giản hóa khái niệm vẫn hữu ích: chúng tôi không giải quyết vấn đề chuỗi chung mà chỉ so sánh căn chỉnh cố định với một mẫu không đổi. 

Người ta có thể tưởng tượng các giải pháp phức tạp hơn bằng cách sử dụng bảng băm hoặc bảng không khớp được tính toán trước, nhưng chúng sẽ chỉ bổ sung thêm chi phí mà không mang lại lợi ích gì. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(10·t) | O(1) | Đã chấp nhận | 
| Tối ưu | O(10·t) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lưu trữ chuỗi tham chiếu “codeforces” một lần trước khi xử lý các trường hợp kiểm thử. Điều này tránh việc phải xây dựng lại hoặc gõ lại nhiều lần và đảm bảo tất cả các so sánh đều nhất quán. 
2. Với mỗi test, đọc chuỗi đầu vào có độ dài 10. 
3. Khởi tạo bộ đếm về 0 cho trường hợp thử nghiệm đó. 
4. Lặp lại tất cả các chỉ số từ 0 đến 9. Tại mỗi chỉ mục, so sánh ký tự trong chuỗi đầu vào với ký tự trong chuỗi tham chiếu. Nếu chúng khác nhau, hãy tăng bộ đếm. 
5. Sau khi hoàn thành hết 10 vị trí, xuất bộ đếm cho test case đó. 

Ý tưởng chính là mọi vị trí đều đóng góp độc lập cho câu trả lời cuối cùng. Không có sự tương tác giữa các chỉ số, vì vậy chỉ cần một lần chuyển là đủ. 

### Tại sao nó hoạt động 

Mỗi vị trí trong chuỗi là một điều kiện nhị phân độc lập: các ký tự khớp hoặc không. Câu trả lời cuối cùng chỉ đơn giản là tổng của các chỉ số không phù hợp độc lập này. Vì chuỗi tham chiếu là cố định và chuỗi đầu vào không được sửa đổi nên việc so sánh mang tính xác định và toàn diện trên tất cả các chỉ số. Không có sự sắp xếp thay thế hoặc phép biến đổi ẩn nào tồn tại, do đó việc đếm các giá trị không khớp trực tiếp tạo ra kết quả chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    target = "codeforces"
    t = int(input())
    
    for _ in range(t):
        s = input().strip()
        diff = 0
        
        for i in range(10):
            if s[i] != target[i]:
                diff += 1
        
        print(diff)

if __name__ == "__main__":
    solve()
```Giải pháp sửa chuỗi tham chiếu một lần và sử dụng lại chuỗi đó cho tất cả các trường hợp thử nghiệm, tránh việc phân bổ lặp lại bên trong vòng lặp. Mỗi trường hợp thử nghiệm được xử lý độc lập và chúng tôi so sánh nghiêm ngặt các ký tự theo chỉ mục, đảm bảo tính chính xác về vị trí. các`.strip()`Điều quan trọng là tránh tính các ký tự dòng mới như một phần của chuỗi, đây là một lỗi tinh vi phổ biến trong I/O lập trình cạnh tranh. 

## Ví dụ đã hoạt động 

Hãy theo dõi hai trường hợp. 

Ví dụ đầu tiên:`coolforsez`so với`codeforces`. 

| tôi | s[i] | mục tiêu[i] | cuộc thi đấu? | khác biệt | 
| --- | --- | --- | --- | --- | 
| 0 | c | c | vâng | 0 | 
| 1 | o | o | vâng | 0 | 
| 2 | o | d | không | 1 | 
| 3 | tôi | e | không | 2 | 
| 4 | f | f | vâng | 2 | 
| 5 | o | o | vâng | 2 | 
| 6 | r | r | vâng | 2 | 
| 7 | s | c | không | 3 | 
| 8 | e | e | vâng | 3 | 
| 9 | z | s | không | 4 | 

Đầu ra cuối cùng là 4. 

Điều này xác nhận rằng sự không khớp được tính hoàn toàn theo vị trí chứ không phải theo tần số ký tự. 

Ví dụ thứ hai:`codeforces`so với chính nó. 

| tôi | s[i] | mục tiêu[i] | cuộc thi đấu? | khác biệt | 
| --- | --- | --- | --- | --- | 
| 0-9 | tất cả đều bình đẳng | tất cả đều bình đẳng | vâng | 0 | 

Điều này thể hiện trường hợp nhận dạng trong đó không xảy ra sự không khớp, xác minh tính đúng đắn của trường hợp cơ sở. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(10·t) | Mỗi trường hợp thử nghiệm so sánh chính xác 10 ký tự một lần | 
| Không gian | O(1) | Chỉ sử dụng chuỗi tham chiếu cố định và bộ đếm | 

Việc tính toán là tuyến tính theo số lượng trường hợp thử nghiệm với hệ số không đổi nhỏ. Với tối đa 1000 đầu vào và chuỗi có độ dài cố định, giải pháp sẽ chạy thoải mái trong cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

def solve():
    target = "codeforces"
    t = int(input())
    for _ in range(t):
        s = input().strip()
        diff = 0
        for i in range(10):
            if s[i] != target[i]:
                diff += 1
        print(diff)

# provided samples
assert run("""5
coolforsez
cadafurcie
codeforces
paiuforces
forcescode
""") == "4\n5\n0\n4\n9"

# custom cases
assert run("""1
codeforces
""") == "0", "all match"

assert run("""1
aaaaaaaaaa
""") == "10", "all mismatch"

assert run("""1
codeforces
""") == "0", "identity check"

assert run("""2
codeforces
codeforces
""") == "0\n0", "repeated identity"

assert run("""1
codeforcez
""") == "1", "single mismatch"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| lực lượng mật mã | 0 | trường hợp cơ sở phù hợp hoàn hảo | 
| aaaaaaa | 10 | tất cả các vị trí đều khác nhau | 
| trường hợp lặp đi lặp lại | 0 0 | xử lý nhiều bài kiểm tra | 
| codeforcez | 1 | vị trí đơn không khớp | 

## Vỏ cạnh 

Trường hợp một cạnh là khi chuỗi đầu vào giống hệt với tham chiếu. Đối với đầu vào`codeforces`, mọi chỉ mục đều khớp, do đó vòng lặp không bao giờ tăng bộ đếm và đầu ra là 0. Thuật toán xử lý việc này một cách tự nhiên vì không có trình kích hoạt có điều kiện. 

Một trường hợp khác là khi mỗi ký tự khác nhau, chẳng hạn như`aaaaaaaaaa`. Ở đây, mỗi phép so sánh trong số 10 phép so sánh đều không thành công và bộ đếm đạt tới 10. Vì chúng tôi lặp lại rõ ràng trên tất cả các chỉ số nên chúng tôi không bỏ sót bất kỳ vị trí nào. 

Một trường hợp tinh vi được lặp lại các đầu vào giống hệt nhau trên nhiều trường hợp thử nghiệm. Vì chúng tôi đặt lại bộ đếm bên trong vòng lặp cho từng trường hợp thử nghiệm nên các kết quả trước đó không bị rò rỉ sang lần tính toán tiếp theo, đảm bảo tính chính xác giữa các đợt.
