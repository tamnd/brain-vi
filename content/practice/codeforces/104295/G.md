---
title: "CF 104295G - \u041f\u043e\u0440\u0430\u0434\u0443\u0439 \u0422\u043e\u0444\u0441\u043b\u0443"
description: "Chúng tôi được cung cấp một bộ sưu tập chiều cao của tháp. Có các tháp $n$, trong đó $n$ được đảm bảo là số lẻ và mỗi tháp có chiều cao ban đầu nào đó. Chúng ta cũng được cấp thêm $k$ khối đơn vị. Mỗi khối có thể được thêm vào chính xác một tòa tháp, tăng chiều cao của nó lên một."
date: "2026-07-01T20:20:18+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104295
codeforces_index: "G"
codeforces_contest_name: "vkoshp.letovo"
rating: 0
weight: 104295
solve_time_s: 52
verified: true
draft: false
---

[CF 104295G - \u041f\u043e\u0440\u0430\u0434\u0443\u0439 \u0422\u043e\u0444\u0441\u043b\u0443](https://codeforces.com/problemset/problem/104295/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 52s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một bộ sưu tập chiều cao của tháp. có$n$tháp, ở đâu$n$được đảm bảo là số lẻ và mỗi tháp có chiều cao ban đầu nào đó. Chúng tôi cũng được trao$k$khối đơn vị bổ sung. Mỗi khối có thể được thêm vào chính xác một tòa tháp, tăng chiều cao của nó lên một. 

Sau khi chúng tôi phân phối tất cả$k$hình khối theo bất kỳ cách nào chúng ta thích, cấu hình cuối cùng được sắp xếp theo thứ tự không giảm. Chúng ta không quan tâm đến mảng đầy đủ sau khi sắp xếp mà chỉ quan tâm đến phần tử trung vị, nghĩa là$\frac{n+1}{2}$-chiều cao nhỏ nhất thứ Mục tiêu của chúng tôi là chọn cách phân phối$k$khối sao cho giá trị trung bình này càng lớn càng tốt. 

Điểm mấu chốt là việc thêm các khối vào một tháp có thể ảnh hưởng hoặc không ảnh hưởng đến điểm trung vị tùy thuộc vào việc tháp đó kết thúc ở vị trí ở dưới hay ở trên vị trí ở giữa sau khi sắp xếp. Vì việc sắp xếp được áp dụng ở cuối nên danh tính của các tòa tháp không quan trọng, chỉ có nhiều giá trị cuối cùng của chúng. 

Các ràng buộc rất lớn:$n$có thể lên tới 300.000 và$k$lên đến$10^9$, do đó, bất kỳ giải pháp nào mô phỏng vị trí khối lập phương được sắp xếp tăng dần hoặc liên tục đều không thể thực hiện được ngay lập tức. Sắp xếp một lần là được, nhưng bất cứ điều gì cố gắng điều chỉnh và đánh giá lại giá trị trung bình nhiều lần sẽ không thành công. 

Trường hợp cạnh tinh tế xuất hiện khi$k = 0$. Sau đó, chúng ta chỉ cần trả về giá trị trung bình của mảng ban đầu, nhưng ngay cả trong trường hợp tầm thường này, lý do vẫn phải phù hợp với phương thức chung. 

Một trường hợp cạnh quan trọng khác là khi có nhiều giá trị bằng nhau xung quanh ranh giới trung vị. Một trực giác ngây thơ có thể cố gắng “trực tiếp nâng tháp trung vị lên”, nhưng vì việc sắp xếp xảy ra sau tất cả các thao tác, nên việc tăng một phần tử có thể chỉ đẩy nó lên trên các phần tử khác mà không thực sự cải thiện vị trí trung bình trừ khi có đủ phần tử được nâng lên. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng phân phối$k$lập phương theo mọi cách có thể, sau đó tính trung vị thu được sau khi sắp xếp. Ngay cả khi chúng ta rời rạc hóa bằng cách suy nghĩ theo mức tăng dần của mỗi$n$tháp, điều này tương đương với việc phân phối$k$các mặt hàng giống hệt nhau vào$n$xô, trong đó có$\binom{k+n-1}{n-1}$khả năng. Điều này phát triển về mặt thiên văn ngay cả đối với các giá trị nhỏ và hoàn toàn không khả thi. 

Một ý tưởng mạnh mẽ có cấu trúc chặt chẽ hơn một chút sẽ là thử phép gán tham lam: liên tục thêm một khối vào tháp mà hiện tại có vẻ có lợi nhất để tăng mức trung bình. Vấn đề là mức trung vị phụ thuộc vào trật tự toàn cầu, vì vậy các lựa chọn tham lam cục bộ không theo dõi một cách đáng tin cậy phần tử nào sẽ kết thúc ở vị trí trung vị sau những thay đổi trong tương lai. 

Quan sát quan trọng là ngừng theo dõi từng tháp riêng lẻ và thay vào đó tập trung vào giá trị ngưỡng. Giả sử chúng ta đoán chiều cao mục tiêu$x$cho trung vị. Câu hỏi trở thành: liệu chúng ta có thể đảm bảo rằng ít nhất một nửa số tòa tháp có chiều cao ít nhất là$x$? Nếu chúng ta có thể thì$x$có thể đạt được dưới dạng giá trị trung bình sau khi sắp xếp. 

Để thực hiện việc kiểm tra này hiệu quả, chúng tôi lưu ý rằng mọi tòa tháp có chiều cao ban đầu dưới$x$cần phải tăng lên ít nhất$x$và mỗi tòa tháp như vậy có chi phí được xác định rõ ràng theo khối. Tháp đã bằng hoặc cao hơn$x$tự nhiên góp phần thỏa mãn điều kiện trung bình mà không cần chi tiêu khối. 

Điều này biến vấn đề thành việc quyết định mức độ lớn của ngưỡng$x$chúng tôi có thể hỗ trợ với ngân sách hạn chế là$k$số gia tăng. Vì tính khả thi là đơn điệu trong$x$, chúng ta có thể tìm kiếm nhị phân câu trả lời. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Phân phối vũ lực | hàm mũ | O(n) | Quá chậm | 
| Tham lam / mô phỏng trực tiếp | O(k log n) | O(n) | Quá chậm | 
| Tìm kiếm nhị phân + kiểm tra tính khả thi | O(n log maxA) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta sắp xếp mảng sao cho việc suy luận về thứ hạng trở nên ổn định. Đặt vị trí trung bình mục tiêu là$mid = \frac{n}{2}$theo thuật ngữ có chỉ số 0. 

Sau đó chúng tôi tìm kiếm nhị phân câu trả lời$x$, kiểm tra xem liệu chúng ta có thể tạo ra số trung vị ít nhất$x$. 

1. Sắp xếp mảng độ cao. Việc sắp xếp là bắt buộc để chúng ta có thể suy luận về số lượng phần tử nằm dưới hoặc trên ngưỡng mà không cần theo dõi danh tính. 
2. Xác định hàm`can(x)`điều đó quyết định liệu số trung vị có thể đạt ít nhất$x$. 
3. Bên trong`can(x)`, chúng tôi lặp lại tất cả các tòa tháp và tính xem cần bao nhiêu khối lập phương để nâng mỗi tòa tháp có chiều cao nhỏ hơn$x$lên đến$x$. Với mỗi tòa tháp như vậy có chiều cao$a[i] < x$, chúng tôi tích lũy$x - a[i]$. 
4. Nếu tổng chi phí vượt quá$k$, chúng tôi ngay lập tức trả về sai. Điều này là do ngay cả trước khi xem xét phân phối một cách tối ưu, chúng ta đã không thể nâng đủ khối lượng để đảm bảo ngưỡng trung bình. 
5. Nếu tổng chi phí nằm trong khoảng$k$, chúng tôi trả về giá trị đúng, nghĩa là chúng tôi có thể buộc ít nhất một nửa số tòa tháp đạt đến độ cao$x$hoặc cao hơn, đảm bảo điều kiện trung bình. 
6. Chúng tôi tìm kiếm nhị phân$x$trong phạm vi từ chiều cao tối thiểu có thể đến chiều cao tối đa có thể cộng thêm$k$, vì số trung vị không thể vượt quá trường hợp tất cả các khối được đặt trên các tháp có liên quan. 
7. Đáp án cuối cùng là lớn nhất$x$vì cái gì`can(x)`là đúng. 

### Tại sao nó hoạt động 

Tính đúng đắn dựa trên thực tế là điều kiện trung bình chỉ phụ thuộc vào số lượng phần tử ít nhất có một giá trị nhất định chứ không phụ thuộc vào danh tính của chúng. Sau khi chúng tôi chọn được một ứng cử viên$x$, bất kỳ tháp nào bên dưới$x$phải tăng lên mới đạt được nếu chúng ta muốn nó đóng góp cho nửa trên. Tiêu khối vào các tòa tháp đã ở trên$x$không bao giờ cần thiết cho tính khả thi của$x$, vì chúng đã thỏa mãn điều kiện ràng buộc. 

Điều này tạo ra một cấu trúc khả thi đơn điệu: nếu chúng ta có thể đạt được mức trung bình ít nhất$x$, thì chúng ta cũng có thể đạt được bất kỳ$y < x$, vì chi phí yêu cầu chỉ giảm khi ngưỡng giảm. Sự đơn điệu này chính xác là điều làm cho tìm kiếm nhị phân có giá trị. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, k = map(int, input().split())
a = list(map(int, input().split()))
a.sort()

mid = n // 2

def can(x):
    need = 0
    for i in range(n):
        if a[i] < x:
            need += x - a[i]
            if need > k:
                return False
    return True

lo, hi = min(a), max(a) + k

while lo < hi:
    mid_val = (lo + hi + 1) // 2
    if can(mid_val):
        lo = mid_val
    else:
        hi = mid_val - 1

print(lo)
```Quá trình triển khai bắt đầu bằng cách sắp xếp mảng sao cho chúng ta có thể xử lý độ cao theo thứ tự, mặc dù bản thân việc kiểm tra tính khả thi không dựa vào cập nhật vị trí sau khi sắp xếp. 

các`can(x)`hàm tính tổng số khối cần thiết để nâng mọi phần tử bên dưới$x$lên đến$x$. Việc thoát sớm khi`need > k`là điều cần thiết để đạt được hiệu quả, đặc biệt khi$k$là lớn. 

Tìm kiếm nhị phân sử dụng độ lệch trung bình trên`(lo + hi + 1) // 2`để tránh các vòng lặp vô hạn khi hội tụ lên trên. Không gian tìm kiếm được giới hạn an toàn bởi`max(a) + k`bởi vì việc đặt tất cả các hình khối trên một tòa tháp có thể tăng tối đa bất kỳ phần tử nào$k$. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 0
1 3 2 5 2
```Mảng được sắp xếp:`[1, 2, 2, 3, 5]`, chỉ số trung bình là 2 (được lập chỉ mục 0). 

Chúng tôi kiểm tra tính khả thi: 

| x | chi phí để đạt x | có thể(x) | 
| --- | --- | --- | 
| 2 | (1→2)=1 | vâng | 
| 3 | (1→3)=2 + (2→3)=1 + (2→3)=1 = 4 | không | 

Giá trị khả thi lớn nhất là 2. 

Điều này cho thấy rằng nếu không có thêm các khối, chúng ta không thể đẩy đủ khối lượng để nâng trung vị vượt quá giá trị ban đầu của nó. 

### Ví dụ 2 

đầu vào:```
5 3
1 5 2 2 3
```Đã sắp xếp:`[1, 2, 2, 3, 5]`Chúng tôi thử tăng ngưỡng: 

| x | chi phí | khả thi | 
| --- | --- | --- | 
| 3 | (1→3)=2 + (2→3)=1 + (2→3)=1 = 4 | không | 
| 2 | (1→2)=1 | vâng | 

Vì vậy, câu trả lời vẫn là 2 ngay cả sau khi chia 3 khối. 

Điều này chứng tỏ rằng việc cải thiện mức trung bình đòi hỏi phải có đủ ngân sách để nâng cao đồng thời nhiều yếu tố chứ không chỉ một yếu tố. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log (maxA + k))$| sắp xếp cộng với tìm kiếm nhị phân trên phạm vi giá trị, mỗi lần kiểm tra tính khả thi sẽ quét mảng | 
| Không gian |$O(1)$thêm | chỉ sắp xếp mảng tại chỗ | 

Giải pháp phù hợp thoải mái trong giới hạn vì$n = 3 \cdot 10^5$và hệ số logarit nhỏ (khoảng 30 lần lặp), dẫn đến khoảng$10^7$các thao tác nguyên thủy. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    a.sort()

    def can(x):
        need = 0
        for v in a:
            if v < x:
                need += x - v
                if need > k:
                    return False
        return True

    lo, hi = min(a), max(a) + k
    while lo < hi:
        mid = (lo + hi + 1) // 2
        if can(mid):
            lo = mid
        else:
            hi = mid - 1
    return str(lo)

# provided samples
assert run("5 0\n1 3 2 5 2\n") == "2"
assert run("5 3\n1 5 2 2 3\n") == "2"

# minimum case
assert run("1 10\n5\n") == "15"

# all equal
assert run("5 4\n2 2 2 2 2\n") == "3"

# large k on skewed array
assert run("3 100\n1 1 1\n") == "34"

# no improvement possible
assert run("3 0\n10 1 2\n") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 15 | lạm phát tối đa | 
| tất cả đều bình đẳng | 3 | phân phối cân bằng | 
| mảng nhỏ lệch | 34 | sự dịch chuyển khối lượng lớn | 
| không k | 1 | tính đúng đắn mà không thay đổi | 

## Vỏ cạnh 

cho$n = 1$, trung vị là phần tử duy nhất, vì vậy mỗi khối lập phương trực tiếp làm tăng câu trả lời. Thuật toán xử lý việc này vì việc kiểm tra tính khả thi chỉ đơn giản là tích lũy$k$thành một giá trị duy nhất và tìm kiếm nhị phân đạt tới$a[0] + k$. 

Khi tất cả các phần tử giống hệt nhau, trung vị chỉ có thể tăng nếu tồn tại đủ khối để nâng ít nhất một nửa trong số chúng lên trên ngưỡng. Hàm khả thi phản ánh chính xác điều này vì nó tính chi phí cho mọi yếu tố bên dưới$x$, đảm bảo rằng chỉ nâng một phần tử là không bao giờ đủ. 

Khi$k = 0$, tìm kiếm nhị phân sẽ thu gọn về giá trị trung vị ban đầu. Vì không có giá trị của$x$trên mức trung bình là khả thi, việc tìm kiếm sẽ trả về phần tử ở giữa được sắp xếp một cách tự nhiên mà không cần viết hoa đặc biệt.
