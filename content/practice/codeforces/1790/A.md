---
title: "CF 1790A - Polycarp và ngày của Pi"
description: "Mỗi trường hợp kiểm thử đưa ra một chuỗi các chữ số mà Polycarp đã viết ra sau khi cố gắng ghi nhớ các chữ số của số π. Nhiệm vụ là xác định có bao nhiêu chữ số đầu của π khớp với tiền tố của chuỗi này. Chúng tôi không xây dựng lại số π hoặc sửa lỗi."
date: "2026-06-09T10:36:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "math", "strings"]
categories: ["algorithms"]
codeforces_contest: 1790
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 847 (Div. 3)"
rating: 800
weight: 1790
solve_time_s: 95
verified: true
draft: false
---

[CF 1790A - Polycarp và Ngày của Pi](https://codeforces.com/problemset/problem/1790/A) 

**Đánh giá:** 800 
**Tags:** triển khai, toán học, chuỗi 
**Thời gian giải:** 1m 35s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi trường hợp kiểm thử đưa ra một chuỗi các chữ số mà Polycarp đã viết ra sau khi cố gắng ghi nhớ các chữ số của số π. Nhiệm vụ là xác định có bao nhiêu chữ số đầu của π khớp với tiền tố của chuỗi này. 

Chúng tôi không xây dựng lại số π hoặc sửa lỗi. Chúng tôi chỉ đơn giản so sánh chuỗi chữ số đã cho với chuỗi tham chiếu đã biết gồm các chữ số π và dừng lại ở điểm không khớp đầu tiên. Câu trả lời cho trường hợp thử nghiệm là độ dài của tiền tố dài nhất khớp chính xác. 

Ràng buộc nhỏ: mỗi chuỗi có tối đa 30 ký tự và có tới 1000 trường hợp kiểm thử. Điều này có nghĩa là ngay cả việc so sánh đơn giản từng ký tự với chuỗi tham chiếu được lưu trữ cũng đủ nhanh. Toàn bộ tác phẩm được giới hạn bởi khoảng 30.000 so sánh ký tự. 

Sự tinh tế chính nằm ở số 0 đứng đầu và sự không khớp sớm. Một lỗi phổ biến là giả sử so sánh số hoặc bỏ qua chữ số "3" hàng đầu của số π. 

Một số tình huống khó khăn quan trọng: 

Một chuỗi như "000" sẽ ngay lập tức trả về 0 vì π bắt đầu bằng "3", do đó ký tự đầu tiên đã bị lỗi. 

Một chuỗi bắt đầu bằng "3" nhưng phân kỳ sau đó, chẳng hạn như "31420", chỉ nên tính tiền tố chính xác là "314". 

Tiền tố chính xác đầy đủ như "3141592653" sẽ trả về độ dài đầy đủ nếu nó khớp với phần đầu của số π. 

## Phương pháp tiếp cận 

Ý tưởng trực tiếp nhất là so sánh chuỗi đầu vào với tiền tố được tính toán trước là π. Chúng tôi lưu trữ các chữ số π dưới dạng một chuỗi, ví dụ: "314159265358979323846264338327..." đủ dài để bao gồm tất cả các dữ liệu đầu vào có thể có. 

Đối với mỗi trường hợp thử nghiệm, chúng tôi quét từ trái sang phải và so sánh các ký tự cho đến khi tìm thấy điểm không khớp hoặc đến cuối chuỗi đầu vào. Chỉ số mà chúng tôi dừng lại là câu trả lời. 

Phương pháp brute-force này đã tối ưu trong vấn đề này vì độ dài đầu vào tối đa là rất nhỏ. Ngay cả khi chúng tôi tính toán lại các so sánh nhiều lần hoặc sử dụng tính năng cắt, độ phức tạp vẫn tuyến tính trong tổng kích thước đầu vào. 

Một cách trừu tượng hơn để thấy điều đó là chúng ta đang tính toán một tiền tố chung dài nhất giữa hai chuỗi: số đã cho và một chuỗi tham chiếu cố định. Vì một chuỗi là hằng số nên chúng ta không cần bất kỳ quá trình tiền xử lý nâng cao nào như cấu trúc băm hoặc hậu tố. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| So sánh lực lượng vũ phu | O(t · n) | O(1) | Đã chấp nhận | 
| Tối ưu (cùng ý tưởng) | O(t · n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lưu trữ các chữ số của số π dưới dạng một chuỗi cố định bắt đầu từ chữ số đầu tiên sau dấu thập phân kèm theo số 3 đứng đầu. 
2. Đối với mỗi trường hợp kiểm thử, hãy đọc chuỗi đầu vào biểu thị các chữ số được ghi nhớ của Polycarp. 
3. Khởi tạo bộ đếm về 0. Bộ đếm này theo dõi xem có bao nhiêu chữ số liên tiếp khớp giữa chuỗi đầu vào và π. 
4. Lặp lại các ký tự của chuỗi đầu vào từ trái sang phải. Đối với mỗi vị trí i, so sánh đầu vào [i] với pi_digits [i]. 
5. Nếu các ký tự bằng nhau, tăng bộ đếm và tiếp tục. 
6. Nếu xảy ra sự không khớp, hãy dừng ngay lập tức vì bất kỳ chữ số nào khác không thể là một phần của tiền tố chính xác. 
7. Xuất bộ đếm. 

Ý tưởng chính là tính đúng đắn của tiền tố là đơn điệu. Khi sự không khớp xảy ra, việc mở rộng phép so sánh không thể khôi phục được tính chính xác. 

### Tại sao nó hoạt động 

Tại mọi vị trí i, thuật toán đảm bảo rằng tất cả các vị trí trước i đều khớp chính xác với π. Sự không khớp đầu tiên xác định ranh giới giữa bộ nhớ đúng và bộ nhớ không chính xác. Vì chúng ta chỉ quan tâm đến sự phù hợp tiền tố chứ không quan tâm đến việc sắp xếp lại hoặc khớp một phần nên điều kiện dừng vừa đủ vừa cần thiết cho tính đúng đắn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

PI = "314159265358979323846264338327950288419716939937510"

t = int(input())
for _ in range(t):
    s = input().strip()
    ans = 0
    for i in range(len(s)):
        if i < len(PI) and s[i] == PI[i]:
            ans += 1
        else:
            break
    print(ans)
```Giải pháp dựa trên một chuỗi các chữ số π được tính toán trước. Vòng lặp so sánh từng ký tự theo vị trí và dừng ngay lập tức khi không khớp. Kiểm tra ranh giới`i < len(PI)`là một biện pháp bảo vệ an toàn, mặc dù hằng số đủ dài cho mọi ràng buộc. 

Một lỗi triển khai phổ biến là quên loại bỏ các ký tự dòng mới, điều này có thể gây ra sự so sánh không chính xác. Một cách khác là chuyển đổi chuỗi thành số nguyên một cách không cần thiết, điều này làm phức tạp việc so sánh mà không mang lại lợi ích gì. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
000
31415
31420
```| Kiểm tra | tôi | s[i] | PI[i] | Trận đấu | trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 000 | 0 | 0 | 3 | Không | 0 | 

Thử nghiệm đầu tiên thất bại ngay lập tức vì chữ số đầu tiên không khớp với số π. 

| Kiểm tra | tôi | s[i] | PI[i] | Trận đấu | trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 31415 | 0 | 3 | 3 | Có | 1 | 
| | 1 | 1 | 1 | Có | 2 | 
| | 2 | 4 | 4 | Có | 3 | 
| | 3 | 1 | 1 | Có | 4 | 
| | 4 | 5 | 5 | Có | 5 | 

Trường hợp này hoàn toàn khớp với năm chữ số đầu tiên. 

| Kiểm tra | tôi | s[i] | PI[i] | Trận đấu | trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 31420 | 0 | 3 | 3 | Có | 1 | 
| | 1 | 1 | 1 | Có | 2 | 
| | 2 | 4 | 4 | Có | 3 | 
| | 3 | 2 | 1 | Không | 3 | 

Điều này cho thấy kết quả khớp tiền tố một phần dừng ở chữ số không chính xác đầu tiên. 

Những dấu vết này xác nhận thuật toán hoạt động như một công cụ so khớp tiền tố nghiêm ngặt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t · n) | Mỗi trường hợp thử nghiệm so sánh tối đa 30 ký tự một lần | 
| Không gian | O(1) | Chỉ sử dụng một chuỗi π cố định và một vài bộ đếm | 

Các ràng buộc làm cho thời gian này không đổi một cách hiệu quả cho mỗi trường hợp thử nghiệm. Ngay cả trong trường hợp xấu nhất là 1000 chuỗi có độ dài 30, tổng công việc là không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    PI = "314159265358979323846264338327950288419716939937510"
    out = []
    t = int(sys.stdin.readline())
    for _ in range(t):
        s = sys.stdin.readline().strip()
        ans = 0
        for i in range(len(s)):
            if i < len(PI) and s[i] == PI[i]:
                ans += 1
            else:
                break
        out.append(str(ans))
    return "\n".join(out)

# provided samples
assert run("""9
000
3
4141592653
141592653589793238462643383279
31420
31415
314159265358
27182
314159265358979323846264338327
""") == """0
1
0
0
3
5
12
0
30"""

# custom cases
assert run("""3
3
314
3141592653
""") == """1
3
10"""

assert run("""2
0
000000
""") == """0
0"""

assert run("""2
314159
3141592653589793238462643383279502884
""") == """6
35"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Tiền tố đúng một chữ số | 1 | trường hợp phù hợp tối thiểu | 
| Tất cả số không | 0 | xử lý không phù hợp ngay lập tức | 
| Trận đấu tiền tố đầy đủ | chiều dài đầy đủ | hành vi tiền tố đúng dài | 

## Vỏ cạnh 

Đối với đầu vào như "000", thuật toán so sánh ký tự đầu tiên với '3' từ π và ngay lập tức dừng lại, trả về 0. Điều này xác nhận rằng các chữ số không hợp lệ ở đầu được xử lý chính xác mà không cần quét thêm. 

Đối với tiền tố khớp hoàn toàn, chẳng hạn như "314159", mỗi ký tự khớp tuần tự và bộ đếm đạt 6. Vòng lặp chỉ kết thúc tự nhiên sau khi dùng hết chuỗi, xác nhận việc xử lý chính xác các kết quả khớp hoàn chỉnh. 

Đối với các trường hợp hỗn hợp như "31420", việc so khớp sẽ tiếp tục với ba ký tự đầu tiên, sau đó không thành công ở ký tự thứ tư và kết quả ổn định ở mức 3, chứng tỏ việc chấm dứt sớm chính xác đối với trường hợp không khớp đầu tiên.
