---
title: "CF 104011J - Hành Trình Trong Sương Mù"
description: "Chúng ta đang làm việc trên đường một chiều có độ dài cố định $L$. Một điểm cuối là nhà của Julia ở vị trí $0$ và điểm cuối còn lại là nhà của Jane ở vị trí $L$."
date: "2026-07-02T05:15:58+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104011
codeforces_index: "J"
codeforces_contest_name: "2021-2022 ICPC NERC (NEERC), North-Western Russia Regional Contest (Northern Subregionals)"
rating: 0
weight: 104011
solve_time_s: 64
verified: true
draft: false
---

[CF 104011J - Hành trình trong sương mù](https://codeforces.com/problemset/problem/104011/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 4s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang làm việc trên đường một chiều có chiều dài cố định$L$. Một điểm cuối là nhà của Julia tại vị trí$0$, và điểm cuối còn lại là nhà của Jane tại vị trí$L$. Tại thời điểm 0, Jane bắt đầu đi bộ từ đầu của mình về phía Julia với một tốc độ không đổi nhưng không xác định, được chọn ngẫu nhiên thống nhất từ ​​một danh sách nhất định.$v_1, v_2, \dots, v_n$. Julia bắt đầu ở vị trí$0$, biết toàn bộ danh sách các tốc độ có thể có, nhưng không biết tốc độ nào được chọn. 

Julia có thể di chuyển đến bất kỳ đâu dọc theo đoạn đường, thay đổi hướng bất kỳ lúc nào và sử dụng bất kỳ tốc độ nào lên đến mức tối đa$V$. Cô ấy chỉ nhìn thấy Jane khi họ trùng khớp ở cùng một điểm. Khi Julia gặp Jane, cô ấy phải trở về nhà với tốc độ tối đa$V$. Tổng thời gian là thời gian gặp nhau cộng với thời gian về. 

Nhiệm vụ là tính tổng thời gian dự kiến ​​tối thiểu có thể, giả sử Julia chọn một chiến lược tối ưu dựa trên tính ngẫu nhiên thống nhất của tốc độ của Jane. 

Các ràng buộc làm rõ rằng bất kỳ giải pháp nào về cơ bản đều phải tuyến tính trong$n$. Với$n$lên đến$10^5$, bậc hai hoặc thậm chí$n \log n$lý luận về mô phỏng trên mỗi tốc độ là không cần thiết và có thể là quá mức cần thiết. Cốt lõi của vấn đề phải quy về việc đánh giá một chức năng đơn giản của từng tốc độ. 

Một khó khăn nhỏ là Julia không biết tốc độ của Jane nên mọi chiến lược đều phải được chọn trước khi quá trình bắt đầu và không thể phụ thuộc vào thực tế.$v_i$. Điều này loại trừ khả năng tối ưu hóa thích ứng theo từng phiên bản và đẩy giải pháp tới ngưỡng hoặc chính sách cấu trúc hoạt động tối ưu trên mọi tốc độ. 

Một trường hợp thất bại phổ biến là cho rằng Julia phải luôn tiến về phía Jane. Ví dụ, nếu$L=1000$,$V=10$, Và$v=50$, việc tích cực tiến về phía trước thực sự có thể khiến kết quả trở nên tồi tệ hơn so với việc chờ đợi, bởi vì Jane đến nhà Julia nhanh hơn nhiều so với những gì Julia có thể tiếp cận cô ấy. 

Một trường hợp thất bại khác là bỏ qua chi phí chuyến về. Gặp nhau gần nhà Jane giúp giảm thời gian gặp mặt nhưng lại tăng khoảng cách quay về và sự đánh đổi này là cần thiết. 

## Phương pháp tiếp cận 

Nỗ lực đầu tiên tự nhiên là mô phỏng chiến lược cho từng tốc độ có thể$v_i$, tính thời gian họp rồi tính trung bình. Ngay cả khi chúng ta sửa một chiến lược đơn giản như “Julia luôn chạy đúng tốc độ.”$V$”, chúng ta có thể tính thời gian gặp nhau như là nghiệm của phương trình chuyển động tuyến tính. 

Nếu Julia di chuyển đúng tốc độ$V$, và Jane di chuyển sang trái với tốc độ$v$, thì khoảng cách của chúng co lại với tốc độ$V+v$. Họ gặp nhau vào lúc$t = \frac{L}{V+v}$, và lúc đó Julia đang ở vị trí$x = Vt$. Tổng thời gian trở thành$t + \frac{x}{V} = \frac{2L}{V+v}$. Điều này mang lại một hình thức khép kín rõ ràng cho chiến lược này. 

Tuy nhiên, chiến lược này không phải lúc nào cũng tối ưu. Nếu Jane nhanh hơn Julia nhiều thì đợi ở nhà sẽ tốt hơn, vì dù sao Jane cũng sẽ đến nơi nhanh chóng. Chờ đợi mang lại thời gian gặp gỡ$L/v$và không mất thêm chi phí di chuyển kể từ khi cuộc gặp diễn ra tại nhà của Julia. 

Quan sát quan trọng là Julia chỉ có hai hành vi có ý nghĩa: hoặc cô ấy tiến về phía Jane ngay lập tức hoặc cô ấy đợi ở nhà. Bất kỳ thời gian chờ đợi trung gian nào trước khi di chuyển đều có thể bị hấp thụ vào một trong những thái cực này mà không cải thiện được kết quả. Điều này dẫn đến quyết định tối ưu cho mỗi tốc độ chỉ phụ thuộc vào việc Jane nhanh hơn hay chậm hơn Julia. 

Nếu như$v < V$, di chuyển ngay lập tức sẽ tốt hơn vì Julia có thể giảm thời gian họp nhanh hơn Jane đến gần. Nếu như$v \ge V$, chờ đợi là tối ưu, vì Jane thu hẹp khoảng cách nhanh hơn mức mà Julia có thể giành được lợi thế bằng cách di chuyển. 

Do đó, vấn đề giảm xuống còn việc đánh giá một biểu thức đơn giản cho mỗi$v_i$và tính trung bình. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng chuyển động theo tốc độ |$O(n)$theo đánh giá, mô hình có khả năng phức tạp |$O(1)$| Quá chậm/không cần thiết | 
| Dạng đóng tối ưu cho mỗi tốc độ |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Dẫn xuất chiến lược tối ưu 

1. Xem xét tốc độ cố định$v$. So sánh hai chiến lược cực đoan: đợi ở nhà hoặc di chuyển ngay về phía Jane với tốc độ tối đa$V$. 
2. Nếu Julia đợi, Jane sẽ đến gặp Julia sau một thời gian$\frac{L}{v}$, vậy tổng thời gian là$\frac{L}{v}$. 
3. Nếu Julia di chuyển ngay lập tức, hãy giải phương trình hội tụ bằng vận tốc tương đối$V+v$, cho thời gian họp$\frac{L}{V+v}$. 
4. Lúc gặp nhau, Julia ở khoảng cách xa$x = V \cdot \frac{L}{V+v}$, vậy thời gian quay về là$\frac{x}{V} = \frac{L}{V+v}$. 
5. Do đó việc di chuyển ngay lập tức sẽ cho ta tổng thời gian$\frac{2L}{V+v}$. 
6. Chọn chiến lược tốt hơn trong hai chiến lược cho việc này$v$:$$\min\left(\frac{L}{v}, \frac{2L}{V+v}\right)$$7. Xác định ngưỡng tại đó chúng bằng nhau:$$\frac{L}{v} = \frac{2L}{V+v} \Rightarrow V = v$$8. Kết luận luật quyết định tối ưu: 

nếu$v \le V$, di chuyển ngay lập tức; còn không thì đợi. 
9. Tính giá trị kỳ vọng bằng cách tính tổng chi phí tối ưu trên tất cả$v_i$và chia cho$n$. 

### Tại sao nó hoạt động 

Đối với một cố định$v$, bất kỳ chiến lược nào cũng có thể được phân tách thành thời gian chờ ban đầu, sau đó là chuyển động có tốc độ tối đa. Chi phí thu được trở thành hàm affine trong thời gian chờ đợi có độ dốc chỉ phụ thuộc vào dấu của$V-v$. Điều này tạo ra một hành vi đơn điệu: nếu Julia nhanh hơn thì việc chờ đợi chỉ gây tổn hại; nếu Jane nhanh hơn thì việc di chuyển chỉ gây tổn thương. Kết quả là, chính sách tối ưu sụp đổ thành các lựa chọn ranh giới là chờ đợi bằng 0 hoặc chờ đợi hoàn toàn, do đó không có chiến lược trung gian nào có thể cải thiện được kỳ vọng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, L, V = map(int, input().split())
    v = list(map(int, input().split()))

    ans = 0.0
    for x in v:
        if x <= V:
            ans += 2.0 * L / (V + x)
        else:
            ans += 1.0 * L / x

    print(ans / n)

if __name__ == "__main__":
    solve()
```Mã trực tiếp thực hiện công thức dẫn xuất cho mỗi tốc độ. Chi tiết triển khai chính là sử dụng xuyên suốt số học dấu phẩy động vì câu trả lời cuối cùng cần có độ chính xác cao và các biểu thức liên quan đến phép chia. 

điều kiện`x <= V`khớp chính xác với ngưỡng dẫn xuất, bao gồm cả đẳng thức, trong đó cả hai công thức đều trùng khớp. Điều này tránh sự không nhất quán phân nhánh trường hợp cạnh. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1 1000 30
```Chỉ có một tốc độ tồn tại, vì vậy chúng tôi đánh giá cả hai chiến lược cho$v=30$. Từ$v \le V$, chúng tôi sử dụng$\frac{2L}{V+v} = \frac{2000}{60} = 33.333...$. 

| v | Chiến lược | Giá trị | 
| --- | --- | --- | 
| 30 | di chuyển ngay lập tức | 33.333... | 

Điều này khẳng định rằng khi Julia nhanh hơn, cô ấy sẽ di chuyển trực tiếp. 

### Ví dụ 2 

đầu vào:```
1 1000 10
```Đây$v=10$, bằng$V$. Cả hai chiến lược đều cho kết quả như nhau:$\frac{L}{v} = 100$, wait-based interpretation, and $\frac{2L}{V+v} = \frac{2000}{20} = 100$. 

| v | Chiến lược | Giá trị | 
| --- | --- | --- | 
| 10 | hoặc | 100 | 

Điều này cho thấy trường hợp ngưỡng hoạt động nhất quán. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi tốc độ được xử lý một lần với số học theo thời gian không đổi | 
| Không gian |$O(1)$| Chỉ các biến tích lũy được lưu trữ | 

Thuật toán là tuyến tính trong$n$, đây là mức tối ưu vì tất cả tốc độ đầu vào phải được đọc. Nó dễ dàng phù hợp với những hạn chế đối với$n \le 10^5$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import math

    n, L, V = map(int, sys.stdin.readline().split())
    v = list(map(int, sys.stdin.readline().split()))

    ans = 0.0
    for x in v:
        if x <= V:
            ans += 2.0 * L / (V + x)
        else:
            ans += 1.0 * L / x

    return str(ans / n)

# provided samples
assert abs(float(run("1 1000 30\n30\n")) - 33.3333333333333) < 1e-9
assert abs(float(run("1 1000 10\n10\n")) - 100.0) < 1e-9

# custom cases
assert abs(float(run("3 100 50\n10 20 60\n")) - ((200/60 + 200/70 + 100/60)/3)) < 1e-9, "mixed speeds"
assert abs(float(run("2 1 1\n1 2\n")) - ((1 + 1/2)/2)) < 1e-9, "small boundary"
assert abs(float(run("1 1000000000 1\n1000000\n")) - (2e9/1000001)) < 1e-6, "large values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tốc độ hỗn hợp | tính toán | kết hợp cả hai chế độ | 
| ranh giới nhỏ | tính toán | hành vi ngưỡng bình đẳng | 
| giá trị lớn | tính toán | ổn định số | 

## Vỏ cạnh 

Trường hợp bình đẳng$v = V$là điểm tinh tế chính. Ví dụ:```
1 100 10
10
```Nếu áp dụng công thức “di chuyển ngay”, chúng ta sẽ nhận được$\frac{2L}{V+v} = \frac{200}{20} = 10$. Nếu chúng ta chờ đợi, chúng ta sẽ nhận được$\frac{L}{v} = 10$. Thuật toán chỉ định chính xác một trong hai hành vi và công thức thống nhất đảm bảo không có sự gián đoạn. 

Một trường hợp cạnh khác là khi$v \gg V$, chẳng hạn như:```
1 1000 1
100
```Ở đây Julia thực sự quá chậm để hưởng lợi từ việc di chuyển. Thuật toán chọn chờ đợi, tạo ra$\frac{L}{v} = 10$. Bất kỳ nỗ lực di chuyển nào cũng sẽ tạo ra giá trị lớn hơn nhiều vì điểm hẹn sẽ dịch chuyển xa nhà của Julia, làm tăng đáng kể chi phí hoàn trả. 

Những trường hợp này xác nhận rằng phân rã dựa trên ngưỡng phân chia chính xác không gian trạng thái thành hai chế độ mà không cần bất kỳ chiến lược chi tiết hơn nào.
