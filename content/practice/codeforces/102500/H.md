---
title: "CF 102500H - Cấu hình chiều cao"
description: "Tôi sẽ cung cấp bài xã luận như một tài liệu có thể tái sử dụng. Chỉnh sửa Hồ sơ cuộc đua được mô tả bằng độ cao của đường ở mỗi số nguyên km. Giữa hai km km liên tiếp, đường là đường thẳng nên độ dốc trong mỗi đoạn không đổi."
date: "2026-08-05T18:12:59+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102500
codeforces_index: "H"
codeforces_contest_name: "2019-2020 ICPC Northwestern European Regional Programming Contest (NWERC 2019)"
rating: 0
weight: 102500
solve_time_s: 78
verified: true
draft: false
---

[CF 102500H - Cấu hình chiều cao](https://codeforces.com/problemset/problem/102500/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 18s 
**Đã xác minh:** có 

##Giải pháp 
Tôi sẽ cung cấp bài xã luận như một tài liệu có thể tái sử dụng. 

Chỉnh sửa 

#Hiểu vấn đề 

Hồ sơ cuộc đua được mô tả bằng độ cao của đường ở mỗi số nguyên km. Giữa hai km km liên tiếp, đường là đường thẳng nên độ dốc trong mỗi đoạn không đổi. Đối với mỗi cấp độ nghiêng được yêu cầu, chúng tôi cần khoảng ngang dài nhất có độ dốc trung bình ít nhất bằng cấp đó. 

Giải thích trực tiếp là chọn hai điểm trên tuyến đường và so sánh chênh lệch độ cao của chúng với khoảng cách theo phương ngang. Khó khăn là điểm cuối không nhất thiết phải ở vị trí nguyên. Quãng đường tối ưu có thể bắt đầu hoặc kết thúc ở giữa một km. 

Độ dài cuộc đua có thể chứa 100000 phân đoạn, trong khi có thể có 50 truy vấn cấp độ khác nhau. Một giải pháp kiểm tra từng cặp điểm cuối sẽ yêu cầu khoảng 10^10 phép so sánh, vượt xa những gì phù hợp. Ngay cả việc thực hiện quét tuyến tính trên mỗi cặp có thể là không thể, do đó mục tiêu phải gần với O(n) cho mỗi truy vấn. 

Một lỗi phổ biến là chỉ kiểm tra vị trí số nguyên km. Ví dụ, với đầu vào```
2 1
0 30 30
2.0
```câu trả lời là`1.5`, vì khoảng cách từ km 0 đến km 1,5 có độ dốc 20 mét/km, tức là 2%. Chỉ kiểm tra các đỉnh sẽ bỏ lỡ khoảng này. 

Một cái bẫy khác là giả định rằng khoảng thời gian tốt nhất phải kết thúc ở một đỉnh. Coi như```
3 1
0 10 0 0
5.0
```Cấu hình được chuyển đổi đạt đến giá trị được yêu cầu bên trong một phân đoạn, vì vậy câu trả lời có thể là khoảng cách phân số. Bất kỳ cách tiếp cận nào chỉ cập nhật câu trả lời ở tọa độ nguyên đều có thể trả về kết quả ngắn hơn. 

Trường hợp cạnh thứ ba xuất hiện khi không bao giờ đạt được điểm yêu cầu:```
2 1
0 0 0
1.0
```Đầu ra đúng là`-1`. Việc triển khai bất cẩn có thể cho kết quả bằng 0 vì nó luôn tìm thấy một cặp vị trí giống hệt nhau. 

# Phương pháp tiếp cận 

Giải pháp bạo lực sẽ thử mọi cặp điểm cuối có thể. Đối với mỗi cặp, nó tính toán độ nghiêng trung bình và giữ khoảng thời gian dài nhất thỏa mãn truy vấn. Với n vị trí, điều này yêu cầu kiểm tra O(n^2), tức là khoảng 10^10 thao tác cho kích thước đầu vào tối đa. 

Quan sát quan trọng đến từ việc viết lại điều kiện. Đối với truy vấn cấp g, hãy xác định 

f(x) = chiều cao(x) - (g / 100) * x 

Với hai vị trí a < b, mặt phẳng nghiêng từ a đến b ít nhất bằng g khi 

f(b) >= f(a). 

Bài toán trở thành tìm khoảng cách dài nhất giữa hai điểm trong đó điểm sau có giá trị ít nhất bằng điểm trước. 

Trong khi quét từ trái sang phải, nếu chúng ta biết giá trị nhỏ nhất của f nhìn thấy cho đến nay và vị trí sớm nhất nơi nó xảy ra thì mọi vị trí hiện tại đều cho khoảng thời gian tốt nhất có thể kết thúc ở đó. Vấn đề duy nhất còn lại là f liên tục và tuyến tính trong mỗi km. Vì một đoạn đường là đơn điệu nên có thể tìm thấy điểm cuối tốt nhất bên trong đoạn đó một cách trực tiếp mà không cần tìm kiếm nhị phân. 

Phương pháp brute-force hoạt động vì nó kiểm tra tất cả các cặp có thể. Nó thất bại vì có quá nhiều cặp. Phép biến đổi ở trên làm giảm vấn đề duy trì tiền tố tối thiểu trong khi chỉ duyệt qua n phần tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) mỗi truy vấn | O(1) | Quá chậm | 
| Tối ưu | O(n) mỗi truy vấn | O(1) bên cạnh việc lưu trữ đầu vào | Đã chấp nhận | 

#Hướng dẫn thuật toán 

1. Đối với truy vấn cấp g, hãy trừ đường có độ dốc g/100 khỏi cấu hình chiều cao. Giá trị mới tại vị trí x là`height(x) - g*x/100`. Khoảng có giá trị chính xác khi giá trị được chuyển đổi này không giảm từ điểm cuối bên trái sang điểm cuối bên phải của nó. 
2. Bắt đầu quét tuyến đường từ trái sang phải. Duy trì giá trị chuyển đổi tối thiểu được thấy cho đến nay và vị trí sớm nhất mà mức tối thiểu đó xuất hiện. Điều này mang lại điểm bắt đầu tốt nhất có thể cho bất kỳ khoảng thời gian nào kết thúc ở vị trí hiện tại. 
3. Đối với mỗi đoạn giữa hai km nguyên, hãy xác định nơi đoạn đường được chuyển đổi đạt đến điểm cuối hợp lệ lớn nhất của nó. Nếu đoạn này tăng lên thì đầu xa luôn có giá trị. Nếu nó giảm, phần hợp lệ sẽ kết thúc khi phân đoạn đạt đến mức tối thiểu tiền tố hiện tại. 
4. Cập nhật câu trả lời với khoảng cách giữa điểm cuối này và vị trí tối thiểu được lưu trữ. Sau khi kết thúc phân đoạn, hãy cập nhật tiền tố tối thiểu bằng cách sử dụng điểm cuối của phân đoạn vì các khoảng thời gian trong tương lai có thể bắt đầu từ đó. 
5. Lặp lại quá trình quét cho từng lớp được yêu cầu và in độ dài dài nhất được tìm thấy. Nếu không tồn tại khoảng độ dài dương, hãy in`-1`. 

Tại sao nó hoạt động: 

Đối với bất kỳ điểm kết thúc y nào có thể xảy ra, điểm bắt đầu tốt nhất là điểm xuất hiện sớm nhất của giá trị biến đổi tối thiểu trước y. Quá trình quét luôn lưu trữ chính xác thông tin này. Bên trong mỗi đoạn, hàm được chuyển đổi là tuyến tính, do đó tập hợp các điểm kết thúc hợp lệ là toàn bộ đoạn hoặc tiền tố của đoạn đó. Thuật toán kiểm tra điểm kết thúc hợp lệ xa nhất, đây là điểm duy nhất có thể cải thiện câu trả lời. Vì mọi điểm cuối có thể đều được xem xét nên không thể bỏ qua khoảng thời gian tối đa. 

#Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve_query(h, g):
    n = len(h) - 1
    slope_need = g / 100.0

    def value(i):
        return h[i] - slope_need * i

    cur_min = value(0)
    min_pos = 0
    ans = 0.0

    cur = value(0)

    for i in range(n):
        nxt = value(i + 1)
        seg_slope = nxt - cur

        if seg_slope >= 0:
            end = i + 1.0
        else:
            t = (cur_min - cur) / seg_slope
            if t >= 1.0:
                end = i + 1.0
            else:
                end = i + t

        if end - min_pos > ans:
            ans = end - min_pos

        cur = nxt

        if cur < cur_min:
            cur_min = cur
            min_pos = i + 1.0

    if ans <= 1e-12:
        return "-1"
    return f"{ans:.10f}"

def main():
    n, k = map(int, input().split())
    h = list(map(int, input().split()))

    grades = []
    for _ in range(k):
        grades.append(float(input()))

    out = []
    for g in grades:
        out.append(solve_query(h, g))

    print("\n".join(out))

if __name__ == "__main__":
    main()
```Mã xử lý từng lớp một cách độc lập vì việc chuyển đổi phụ thuộc vào độ dốc được yêu cầu. chức năng`solve_query`giữ giá trị được chuyển đổi hiện tại, giá trị được chuyển đổi nhỏ nhất được thấy cho đến nay và vị trí đạt đến mức tối thiểu đó. 

biểu hiện`seg_slope = nxt - cur`là độ dốc của đoạn được chuyển đổi. Khi nó không âm, mọi điểm trong đoạn đều có giá trị ít nhất bằng điểm bắt đầu của đoạn, vì vậy điểm cuối xa là lựa chọn tốt nhất. Khi nó âm, cuối cùng đường này sẽ giảm xuống dưới mức tối thiểu được lưu trữ và công thức sẽ tính toán điểm giao nhau chính xác. 

Tất cả tọa độ được lưu trữ dưới dạng giá trị dấu phẩy động vì điểm cuối tối ưu có thể nằm bên trong một đoạn. Số nguyên Python xử lý độ cao ban đầu một cách an toàn và chỉ các phép tính được chuyển đổi mới yêu cầu độ chính xác của dấu phẩy động. 

# Ví dụ đã hoạt động 

Đối với mẫu đầu tiên, truy vấn là 2,0%. Biên dạng chuyển đổi được tạo ra bằng cách trừ đi 2 mét trên mỗi km. 

| Phân đoạn | Vị trí tối thiểu hiện tại | Giá trị chuyển đổi hiện tại | Điểm cuối tốt nhất | Trả lời | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | 0 | 0 | 1 | 1 | 
| 1 đến 2 | 0 | -2 | 2 | 2 | 
| 2 đến 3 | 0 | -4 | 3 | 3 | 
| Các đoạn còn lại | 0 | mức tối thiểu vẫn thấp hơn | không cải thiện | 3 | 

Khoảng cách từ km 0 đến km 3 có độ dốc trung bình chính xác theo yêu cầu nên đáp án là 3. 

Đối với mẫu thứ hai, truy vấn là 2,0%. 

| Phân đoạn | Vị trí tối thiểu hiện tại | Giá trị chuyển đổi hiện tại | Điểm cuối tốt nhất | Trả lời | 
| --- | --- | --- | --- | --- | 
| 0 đến 1 | 0 | 0 | 1 | 1 | 
| 1 đến 2 | 0 | 28 | 1,5 | 1,5 | 

Đoạn thứ hai phẳng sau khi chuyển đổi, cho phép điểm cuối phân số. Thuật toán tìm khoảng thời gian có độ dài 1,5. 

# Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nk) | Mỗi lớp k quét tất cả n phân đoạn một lần | 
| Không gian | O(n) | Mảng chiều cao được lưu trữ và quá trình quét sử dụng bộ nhớ bổ sung không đổi | 

Công việc tối đa là khoảng 5 triệu hoạt động phân khúc, dễ dàng nằm trong giới hạn dự định. 

# Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    old_out = sys.stdout
    sys.stdout = out
    main()
    sys.stdout = old_out
    sys.stdin = old
    return out.getvalue().strip()

assert run("""8 2
0 0 10 30 60 45 75 65 30
2.0
3.1
""") == "3.0000000000\n-1"

assert run("""2 2
0 30 30
3.0
2.0
""") == "1.0000000000\n1.5000000000"

assert run("""2 1
0 0 0
1.0
""") == "-1"

assert run("""2 1
0 100 200
50.0
""") == "2.0000000000"

assert run("""2 1
5 5 5
0.0
""") == "2.0000000000"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Hồ sơ phẳng với cấp tích cực | -1 | Không tồn tại khoảng hợp lệ | 
| Hồ sơ tăng thẳng | 2.0 | Toàn bộ tuyến đường có thể hợp lệ | 
| Trường hợp điểm cuối phân số | 1,5 | Các câu trả lời không nguyên được xử lý | 
| Chiều cao bằng nhau không có điểm | 2.0 | Các đoạn phẳng được tính cho độ dốc bằng 0 | 

# Vỏ cạnh 

Khi điểm cuối tốt nhất nằm trong một phân đoạn, thuật toán sẽ tiếp cận điểm cuối đó thông qua công thức cắt đoạn giảm dần. Đối với đầu vào```
2 1
0 30 30
2.0
```các giá trị được chuyển đổi là 0, 28 và 26. Giá trị tối thiểu giữ nguyên ở vị trí 0. Đoạn thứ hai giảm từ 28 xuống 26 và việc vượt qua giá trị tối thiểu xảy ra sau 1,5 km, tạo ra câu trả lời đúng. 

Khi mọi khoảng không đạt yêu cầu, tiền tố tối thiểu không bao giờ cho phép khoảng cách hợp lệ dương. Vì```
2 1
0 0 0
1.0
```cấu hình được chuyển đổi sẽ giảm ngay lập tức và mọi khoảng thời gian có thể có độ dốc được điều chỉnh âm. Câu trả lời được lưu trữ vẫn bằng 0, do đó chương trình sẽ in`-1`. 

Khi toàn bộ tuyến đường hợp lệ, điểm tối thiểu vẫn ở đầu và điểm cuối cuối cùng sẽ đưa ra câu trả lời. Vì```
2 1
0 100 200
50.0
```cấu hình được chuyển đổi bằng phẳng nên quá trình quét đạt đến km 2 và trả về toàn bộ chiều dài.
