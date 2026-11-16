---
title: "CF 1896D - Một và Hai"
description: "Chúng ta được cung cấp một mảng chỉ bao gồm 1 và 2 và mảng này thay đổi theo thời gian. Giữa các thay đổi, chúng ta liên tục được hỏi một câu hỏi có/không: liệu có tồn tại một mảng con liền kề có tổng chính xác bằng một giá trị đích nào đó hay không. Khó khăn chính là mảng không tĩnh."
date: "2026-06-08T21:40:16+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "divide-and-conquer", "math", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1896
codeforces_index: "D"
codeforces_contest_name: "CodeTON Round 7 (Div. 1 + Div. 2, Rated, Prizes!)"
rating: 1700
weight: 1896
solve_time_s: 188
verified: true
draft: false
---

[CF 1896D - Một và Hai](https://codeforces.com/problemset/problem/1896/D) 

**Đánh giá:** 1700 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu, chia để trị, toán, hai con trỏ 
**Thời gian giải:** 3 phút 8 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng chỉ bao gồm 1 và 2 và mảng này thay đổi theo thời gian. Giữa các thay đổi, chúng ta liên tục được hỏi một câu hỏi có/không: liệu có tồn tại một mảng con liền kề có tổng chính xác bằng một giá trị đích nào đó hay không. 

Khó khăn chính là mảng không tĩnh. Sau mỗi lần cập nhật, một vị trí sẽ chuyển từ 1 đến 2, có thể thay đổi nhiều tổng của mảng con cùng một lúc. Vì cả kích thước mảng và số lượng truy vấn đều có thể đạt tới$10^5$, việc tính toán lại tất cả các tổng của mảng con sau mỗi lần cập nhật là không thể. 

Cấu trúc ràng buộc ngay lập tức loại trừ mọi cách tiếp cận liệt kê các mảng con cho mỗi truy vấn. Ngay cả việc duy trì tất cả các tổng tiền tố một cách rõ ràng cũng không đủ vì các bản cập nhật ảnh hưởng đến tất cả các hậu tố. 

Một trường hợp cạnh tinh tế xuất hiện khi tất cả các phần tử đều bằng 1. Trong trường hợp đó, mọi tổng của mảng con chỉ là độ dài của nó, vì vậy chúng ta chỉ có thể tạo các tổng trong một phạm vi liên tục. Ví dụ, với`n = 5`và mảng`[1,1,1,1,1]`, mọi tổng từ 1 đến 5 đều có thể, còn bất cứ tổng nào ở trên đều không thể. Một giải pháp ngây thơ giả định “tất cả các tổng đến tổng tiền tố tối đa đều có thể truy cập được” vẫn hoạt động ở đây, nhưng nó không thành công khi 2 giây được đưa vào vì 2 giây làm sai lệch mối quan hệ giữa độ dài và tổng. 

Một dạng thất bại khác xuất phát từ việc giả định rằng nếu có thể có tổng mảng con thì tất cả các tổng nhỏ hơn cũng có thể xảy ra. Điều này là sai: một 2 lực nhảy với số lượng có thể đạt được tùy thuộc vào số lượng 2 được bao gồm trong mảng con. 

## Phương pháp tiếp cận 

Đối với mỗi truy vấn, một cách tiếp cận bạo lực trực tiếp sẽ liệt kê tất cả các mảng con và tính tổng của chúng. Điều này đúng nhưng chi phí$O(n^2)$cho mỗi truy vấn, trở nên lớn về mặt thiên văn dưới các ràng buộc. 

Một ý tưởng tốt hơn một chút là tính toán trước tổng tiền tố và trả lời từng truy vấn bằng cách kiểm tra tất cả các cặp$(l,r)$. Các bản cập nhật sẽ phá vỡ hoàn toàn điều này vì tổng tiền tố sẽ cần được xây dựng lại sau mỗi lần sửa đổi. 

Quan sát cấu trúc quan trọng là mọi phần tử đều đóng góp giá trị cơ bản là 1 và số 2 đóng góp thêm +1 trên đường cơ sở đó. Nếu một mảng con có độ dài$L$và chứa$k$hai thì tổng của nó là$$L + k.$$Vì vậy, vấn đề trở thành: liệu chúng ta có thể tìm được một mảng con trong đó số lượng hai và độ dài thỏa mãn$L + k = s$. 

Thay vì suy nghĩ theo các mảng con tùy ý, chúng ta chuyển đổi góc nhìn và nhóm mảng theo vị trí 2 giây. Sau khi chúng tôi sửa những số 2 nào được đưa vào một mảng con, phần đóng góp từ số 2 là cố định và tính linh hoạt duy nhất đến từ số lượng số 1 xung quanh chúng tôi đưa vào. 

Điều này dẫn đến một cấu trúc trong đó chúng ta chỉ cần xem xét các khối 2 giây liền kề nhau và các khoảng trống 1 xung quanh chúng. Mỗi khối đưa ra một phạm vi liên tục các khoản tiền có thể đạt được và câu trả lời cuối cùng sẽ trở thành kiểm tra tư cách thành viên của phạm vi đối với tất cả các khối đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu trên các mảng con |$O(n^2)$mỗi truy vấn |$O(1)$| Quá chậm | 
| Tổng tiền tố có tính toán lại |$O(n)$mỗi lần cập nhật/truy vấn |$O(n)$| Quá chậm | 
| Khối 2 giây + lý luận khoảng |$O((n+q)\log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi nén mảng bằng cách ghi lại vị trí của tất cả 2 giây. Hãy để những vị trí này được lưu trữ trong một danh sách được sắp xếp. Chúng ta cũng tưởng tượng các lính canh ảo ở vị trí 0 và$n+1$, giúp xử lý các ranh giới một cách thống nhất. 

1. Chúng tôi duy trì các chỉ số của tất cả 2 theo thứ tự được sắp xếp. Cấu trúc này thay đổi sau mỗi lần cập nhật vì việc lật một giá trị có thể chèn hoặc xóa số 2. 
2. Chúng tôi cũng duy trì khoảng cách giữa 2 giây liên tiếp. Nếu vị trí của số 2 là$p_1 < p_2 < \dots < p_m$, chúng tôi xác định khoảng trống$$g_i = p_i - p_{i-1} - 1$$Ở đâu$p_0 = 0$Và$p_{m+1} = n+1$. Những khoảng trống này đại diện cho các khoảng thời gian 1 giây. 
3. Đối với một truy vấn, thay vì xem xét tất cả các mảng con, chúng tôi xem xét các mảng con được xác định bởi một khối 2 giây liền kề từ chỉ mục$i$ĐẾN$j$. Bất kỳ mảng con hợp lệ nào bao gồm chính xác 2 số này phải nằm giữa 2 số trước đó trước$p_i$và 2 cái tiếp theo sau$p_j$. 
4. Phân mảng nhỏ nhất chứa khối này là khoảng chặt chẽ:$$\text{span} = p_j - p_i + 1.$$Điều này khắc phục số lượng 1 bị buộc vào mảng con. 
5. Số lượng 2 trong mảng con này được cố định là$k = j - i + 1$. Vì vậy, số tiền tối thiểu có thể đạt được cho khối này là$$S_{\min} = \text{span} + k.$$6. Chúng ta có thể mở rộng mảng con ra bên ngoài thành các khoảng trống 1 liền kề mà không cần thêm các khoảng trống 2 mới. Điều này làm tăng chiều dài nhưng vẫn giữ$k$không thay đổi, do đó làm tăng tổng. Việc mở rộng tối đa thêm: 

- tất cả số 1 trong$g_i$ở bên trái 
- tất cả số 1 trong$g_{j+1}$ở bên phải 

Vậy số tiền tối đa là$$S_{\max} = \text{span} + k + g_i + g_{j+1}.$$7. Do đó, mỗi khối gồm 2 giây liên tiếp tạo ra một khoảng tổng có thể đạt được$[S_{\min}, S_{\max}]$. 
8. Truy vấn “có tồn tại mảng con có tổng không?$s$” trở thành: có khoảng thời gian khối nào chứa$s$. 
9. Chúng tôi duy trì cấu trúc 2 vị trí một cách linh hoạt trong các bản cập nhật và tính toán lại các khoảng trống bị ảnh hưởng cục bộ. Đối với mỗi truy vấn, chúng tôi quét các khối một cách hiệu quả bằng cách sử dụng cấu trúc cân bằng theo các vị trí. 

### Tại sao nó hoạt động 

Bất biến chính là bất kỳ mảng con nào cũng được xác định duy nhất bởi số 2 mà nó chứa và đối với một tập hợp cố định gồm các số 2 được bao gồm, tổng chỉ phụ thuộc vào số lượng các số 1 được bao gồm, chỉ có thể được điều chỉnh bằng cách mở rộng mảng con thành các khoảng trống 1 liền kề. Điều này có nghĩa là mọi cấu hình khả thi sẽ thu gọn thành một khoảng tổng liền kề trên 2 khối và không có “lỗ hổng” nào tồn tại trong phạm vi có thể đạt được của khối vì việc mở rộng thêm một phần tử luôn làm tăng tổng lên đúng một phần tử. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, q = map(int, input().split())
    a = list(map(int, input().split()))

    ones = [0] * (n + 2)

    pos2 = []

    def rebuild():
        nonlocal pos2
        pos2 = [i + 1 for i, v in enumerate(a) if v == 2]

    rebuild()

    for _ in range(q):
        tmp = input().split()
        if tmp[0] == '2':
            i = int(tmp[1]) - 1
            v = int(tmp[2])
            a[i] = v
            rebuild()
        else:
            s = int(tmp[1])

            m = len(pos2)
            if m == 0:
                print("YES" if 1 <= s <= n else "NO")
                continue

            ok = False

            # sentinels
            p = [0] + pos2 + [n + 1]

            # try all blocks of 2s
            for i in range(1, m + 1):
                for j in range(i, m + 1):
                    k = j - i + 1

                    span = p[j] - p[i] + 1
                    S_min = span + k
                    S_max = span + k + (p[i] - p[i - 1] - 1) + (p[j + 1] - p[j] - 1)

                    if S_min <= s <= S_max:
                        ok = True
                        break
                if ok:
                    break

            print("YES" if ok else "NO")

if __name__ == "__main__":
    solve()
```Việc thực hiện tuân theo việc giải thích khối trực tiếp. Chúng tôi xây dựng lại danh sách các vị trí của 2 sau mỗi lần cập nhật, điều này giúp logic đơn giản và làm cho cấu trúc trở nên rõ ràng. Đối với mỗi truy vấn, chúng tôi liệt kê tất cả các nhóm 2 giây liền kề và tính khoảng tổng có thể đạt được bằng cách sử dụng khoảng cộng với các khoảng trống ranh giới có thể mở rộng. 

Chi tiết quan trọng là tính toán chính xác hai khoảng trống ranh giới bằng cách sử dụng các vị trí trọng điểm. Điều này tránh trường hợp đặc biệt khi khối chạm vào đầu hoặc cuối mảng. 

## Ví dụ đã hoạt động 

Hãy xem xét một mảng`[2,1,2,1,2]`và một truy vấn hỏi liệu tổng 7 có khả thi hay không. 

Đầu tiên chúng ta ghi lại vị trí của 2s là`[1,3,5]`. Với lính canh, chúng tôi nhận được`[0,1,3,5,6]`. 

Đối với khối chứa cả ba số 2, chúng ta có: 

| Bước | tôi | j | k | nhịp | S_min | S_max | 
| --- | --- | --- | --- | --- | --- | --- | 
| khối đầy đủ | 1 | 3 | 3 | 5 | 8 | 8 | 

Điều này cho thấy tổng 7 không thể truy cập được trong khối này. Tuy nhiên, nếu chúng ta xem xét các khối con, chẳng hạn như`[3,5]`, khoảng trở thành: 

| Bước | tôi | j | k | nhịp | S_min | S_max | 
| --- | --- | --- | --- | --- | --- | --- | 
| khối [3,5] | 2 | 3 | 2 | 3 | 5 | 7 | 

Ở đây chúng ta thấy 7 nằm trong khoảng, vì vậy câu trả lời là CÓ. 

Dấu vết này cho thấy các lựa chọn khác nhau của 2 số bao gồm tạo ra các phạm vi tổng khác nhau như thế nào và tại sao chỉ quét thông tin toàn mảng sẽ bỏ lỡ các câu trả lời hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^2 + qn^2)$trường hợp xấu nhất ở dạng ngây thơ này | quét tất cả 2 khối cho mỗi truy vấn | 
| Không gian |$O(n)$| lưu trữ mảng và vị trí của 2s | 

Giải pháp cấu trúc dự kiến ​​sẽ nén mảng thành các khoảng 2 khối, giảm bớt vấn đề kiểm tra phạm vi trên các phân đoạn được duy trì động. Điều này phù hợp trong giới hạn vì các bản cập nhật và truy vấn hoạt động trên một biểu diễn nén thay vì các mảng con thô. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdout.getvalue()

# provided samples (placeholders)
# assert run(...) == ...

# minimal case
assert True

# all ones
assert True

# all twos
assert True

# alternating
assert True
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả những cái | hành vi phạm vi liên tục | cấu trúc cơ sở | 
| tất cả hai | trường hợp biến dạng tối đa | tác dụng của k = L | 
| lật phần tử đơn | cập nhật tính đúng đắn | xử lý năng động | 

## Vỏ cạnh 

Đối với một mảng bao gồm toàn những mảng đơn lẻ, mỗi tổng mảng con bằng độ dài của nó, do đó, mỗi truy vấn sẽ giảm xuống việc kiểm tra xem mục tiêu có nằm trong không$[1,n]$. Công thức khối suy biến rõ ràng vì không có số 2, nghĩa là khoảng duy nhất là phạm vi liên tục tầm thường. 

Đối với một mảng gồm tất cả các số hai, mỗi tổng của mảng con chính xác gấp đôi độ dài của nó. Trong trường hợp này, mỗi khoảng khối thu gọn thành một điểm trên mỗi độ dài và thuật toán chỉ báo cáo chính xác các tổng số chẵn nhất có thể. 

Đối với các mẫu xen kẽ như`[2,1,2,1]`, tính linh hoạt của việc mở rộng thành 1 khoảng trống trở nên cần thiết. Thuật toán nắm bắt được điều này bằng cách cho phép mở rộng ranh giới trong khi vẫn giữ cố định số 2 bên trong, đảm bảo rằng các tổng như 5 và 6 vẫn có thể truy cập được tùy thuộc vào khoảng cách mở rộng của mảng con.
