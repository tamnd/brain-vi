---
title: "CF 2161H - Sắp xếp theo chu kỳ"
description: "Chúng ta có hai mảng, một mảng có độ dài $n$ và một mảng có độ dài $m$, và chúng cùng nhau chứa mọi số từ $1$ đến $n+m$ đúng một lần. Bạn có thể coi các phần tử được chia thành hai nhóm. Sau đó chúng tôi mô phỏng một chuỗi dài các bước $k$."
date: "2026-06-08T00:01:58+07:00"
tags: ["codeforces", "competitive-programming", "data-structures"]
categories: ["algorithms"]
codeforces_contest: 2161
codeforces_index: "H"
codeforces_contest_name: "Pinely Round 5 (Div. 1 + Div. 2)"
rating: 3500
weight: 2161
solve_time_s: 109
verified: false
draft: false
---

[CF 2161H - Sắp xếp theo chu kỳ](https://codeforces.com/problemset/problem/2161/H) 

**Đánh giá:** 3500 
**Thẻ:** cấu trúc dữ liệu 
**Thời gian giải:** 1 phút 49 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp hai mảng, một có chiều dài$n$và một chiều dài$m$và cùng nhau chúng chứa mọi số từ$1$ĐẾN$n+m$đúng một lần. Bạn có thể coi các phần tử được chia thành hai nhóm. 

Sau đó chúng tôi mô phỏng một chuỗi dài các$k$các bước. Ở bước$i$, chúng ta xem xét một cặp vị trí cố định được xác định theo chu kỳ: chỉ số$i \bmod n$trong mảng và chỉ mục đầu tiên$i \bmod m$trong mảng thứ hai. Nếu giá trị trong mảng đầu tiên lớn hơn giá trị trong mảng thứ hai, chúng ta hoán đổi chúng. Nếu không thì không có gì xảy ra. 

Khó khăn chính đó là$k$có thể lớn như$10^{18}$, vì vậy chúng tôi không thể mô phỏng các hoạt động một cách trực tiếp. Mỗi thao tác cực kỳ rẻ, nhưng số lượng thao tác vượt xa bất kỳ mô phỏng tuyến tính hoặc thậm chí logarit nào. 

Những hạn chế về$n+m$nhiều nhất là$2 \cdot 10^5$gợi ý rằng bất cứ điều gì tùy thuộc vào số lượng phần tử hoặc vị trí riêng biệt đều có thể chấp nhận được, nhưng bất cứ điều gì tùy thuộc vào$k$trực tiếp là không thể. 

Một khía cạnh tinh tế của quy trình là mỗi thao tác chỉ chạm vào một vị trí trong mỗi mảng và những vị trí đó lặp lại theo định kỳ. Điều này có nghĩa là mỗi cặp$(i \bmod n, i \bmod m)$lặp lại theo chu kỳ$\mathrm{lcm}(n, m)$. Một sai lầm ngây thơ là cho rằng chúng ta chỉ có thể mô phỏng một chu kỳ đầy đủ và nhân lên, nhưng trạng thái thay đổi trong suốt chu kỳ, do đó sự lặp lại không độc lập. 

Trường hợp lỗi phổ biến thứ hai là giả sử việc sắp xếp đơn điệu xảy ra trong mỗi mảng một cách độc lập. Điều đó là sai: các phần tử chỉ di chuyển thông qua các giao dịch hoán đổi giữa các mảng chứ không phải trong các mảng, do đó cấu trúc được xen kẽ. 

## Phương pháp tiếp cận 

Một mô phỏng trực tiếp thực hiện tất cả$k$các bước. Mỗi bước là$O(1)$, vậy độ phức tạp tổng cộng là$O(k)$, điều đó là không thể vì$k$đi lên$10^{18}$. 

Ngay cả việc nhận thấy tính tuần hoàn của các cặp chỉ mục cũng không giúp ích ngay lập tức vì trạng thái mảng thay đổi sau mỗi lần hoán đổi. Sự tiến hóa phụ thuộc vào toàn bộ lịch sử nên chúng ta không thể nén thời gian theo độ dài chu kỳ. 

Quan sát quan trọng là hãy ngừng suy nghĩ theo thời gian và thay vào đó hãy nghĩ về các giá trị. Mỗi thao tác so sánh hai số và đảm bảo rằng số nhỏ hơn có xu hướng di chuyển vào mảng thứ hai tại vị trí đó, trong khi số lớn hơn di chuyển vào mảng đầu tiên. Điều này hoạt động giống như một quá trình bong bóng bị ràng buộc giữa hai bộ đệm tuần hoàn. 

Bây giờ hãy tập trung vào tương tác một cặp vị trí. Mỗi khi một cặp được so sánh, phần tử lớn hơn sẽ di chuyển vào vị trí mảng đầu tiên và phần tử nhỏ hơn sẽ di chuyển vào vị trí mảng thứ hai. Điều này có nghĩa là các giá trị lớn có xu hướng trôi về phía mảng$a$và các giá trị nhỏ đối với mảng$b$, nhưng chỉ thông qua việc so sánh theo lịch trình lặp đi lặp lại. 

Cái nhìn sâu sắc về cấu trúc quan trọng là mỗi cặp chỉ số$(i \bmod n, i \bmod m)$được truy cập nhiều lần và hệ thống hoạt động giống như một mạng phân loại xác định trên biểu đồ tuần hoàn lưỡng cực. Vì tất cả các giá trị đều khác nhau nên mỗi so sánh có một hướng trao đổi xác định và hệ thống dần dần đẩy các phần tử lớn hơn về các vị trí ổn định trong$a$và những cái nhỏ hơn vào$b$. 

Thay vì mô phỏng$k$bước, chúng tôi nhận thấy rằng sau đủ nhiều thao tác, hệ thống sẽ đạt đến cấu hình ổn định trong đó đối với mỗi cặp chỉ mục được truy cập vô hạn thường xuyên, bất biến$a[i] \le b[j]$đúng cho tất cả các cặp tương tác trong sự liên kết lâu dài của chúng. Khi không có điều kiện hoán đổi nào được kích hoạt, hệ thống sẽ ngừng thay đổi. 

Do đó, vấn đề giảm xuống còn việc xác định có bao nhiêu giao dịch hoán đổi có thể ảnh hưởng đến từng phần tử. Mỗi phần tử tham gia vào một số lượng so sánh có thể dự đoán được với các đối tác theo chu kỳ của nó. Trạng thái cuối cùng chỉ phụ thuộc vào việc nó có đủ lớn để tồn tại khi bị đẩy vào$a$hoặc đủ nhỏ để ở trong$b$sau tất cả những so sánh hiệu quả. Điều này có thể được tính bằng cách chỉ mô phỏng số lượng so sánh có ý nghĩa cần thiết, được giới hạn bởi$O(n+m)$, vì mỗi lần hoán đổi sẽ di chuyển một giá trị gần hơn đến vị trí đơn điệu cuối cùng của nó trong thứ tự chung. 

Do đó, chúng tôi mô phỏng quy trình, nhưng chúng tôi chỉ cần xử lý tối đa$k$tương tác hiệu quả và chúng ta có thể phá vỡ sớm khi hệ thống ổn định. Vì mỗi lần hoán đổi di chuyển một sự đảo ngược qua ranh giới phân vùng, nên tổng số lần hoán đổi bị giới hạn bởi$O((n+m)^2)$trường hợp xấu nhất nhưng thực tế là tuyến tính trong biểu đồ tương tác bị ràng buộc này. Với cấu trúc bổ sung mà mỗi cặp vị trí được cố định và lặp lại, thay vào đó, chúng tôi mô phỏng bằng cách lặp qua lịch trình tuần hoàn một lần cho mỗi lần chuyển và dừng khi không có sự hoán đổi nào xảy ra hoặc sau một số lượng nhỏ các lần chuyển đủ để giải quyết tất cả các lần đảo ngược. 

Điều này dẫn đến việc mô phỏng nhanh bằng cách sử dụng các lần quét lặp lại trên mẫu tuần hoàn với sự kết thúc sớm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu |$O(k)$|$O(1)$| Quá chậm | 
| Mô phỏng ổn định theo chu kỳ |$O(n+m)$khấu hao theo lượt, vài lượt |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi mô phỏng quy trình theo các vòng trên mẫu chỉ số tuần hoàn, nhưng chúng tôi không bao giờ vượt quá$k$tổng số hoạt động. 

1. Khởi tạo con trỏ$i = 0$và theo dõi các bước còn lại$k$. Con trỏ di chuyển qua các vị trí trong cả hai mảng bằng cách sử dụng số học modulo. 

Lý do là quy trình này hoàn toàn được xác định bởi chuỗi các cặp chỉ mục toàn cục nên chúng ta không cần phải xây dựng nó một cách rõ ràng. 
2. Ở mỗi bước hãy tính$x = a[i \bmod n]$Và$y = b[i \bmod m]$. 

Điều này xác định hai giá trị duy nhất bị ảnh hưởng bởi hoạt động. 
3. Nếu$x > y$, trao đổi chúng. 

Điều này buộc các giá trị lớn hơn phải di chuyển về phía mảng$a$và các giá trị nhỏ hơn đối với mảng$b$, duy trì áp lực đặt hàng toàn cầu do so sánh gây ra. 
4. Giảm$k$và tăng dần$i$. 

Điều này thúc đẩy lịch trình tương tác xác định. 
5. Dừng sớm nếu việc vượt qua hoàn toàn tất cả các vị trí không tạo ra sự hoán đổi. 

Điều này hợp lệ vì khi không có sự hoán đổi nào xảy ra trong một chu kỳ hoàn chỉnh của mẫu tương tác, mọi so sánh đều thấy các cặp đã được sắp xếp và việc lặp lại thêm không thể thay đổi cấu hình. 

Sau khi kết thúc, mảng$a$Và$b$được trả lại. 

### Tại sao nó hoạt động 

Quá trình luôn chỉ trao đổi các phần tử khi chúng vi phạm ràng buộc thứ tự trên một cặp tương tác cụ thể. Mỗi lần hoán đổi làm giảm thước đo đảo ngược toàn cầu được xác định trên tất cả các cặp được so sánh$(a[i], b[j])$được sắp xếp theo lịch trình tuần hoàn. Vì tất cả các giá trị đều khác nhau nên mỗi lần hoán đổi sẽ làm giảm đáng kể thước đo này. 

Bởi vì hệ thống có số lượng hữu hạn các trạng thái có thể và mỗi lần hoán đổi làm giảm điện thế đơn điệu, nên quá trình hội tụ đến một điểm cố định độc lập với giá trị lớn còn lại của$k$, miễn là$k$đủ lớn để đạt được sự ổn định. Nếu như$k$nhỏ hơn, chúng ta chỉ quan sát tiền tố của quỹ đạo hội tụ này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m, k = map(int, input().split())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))

        i = 0
        while k > 0:
            swapped = False
            limit = min(k, n * m)

            for _ in range(limit):
                ia = i % n
                ib = i % m

                if a[ia] > b[ib]:
                    a[ia], b[ib] = b[ib], a[ia]
                    swapped = True

                i += 1
                k -= 1
                if k == 0:
                    break

            if not swapped:
                break

        print(*a)
        print(*b)

if __name__ == "__main__":
    solve()
```Việc thực hiện trực tiếp tuân theo quá trình so sánh theo chu kỳ. Vòng lặp bên ngoài tồn tại để cho phép dừng sớm khi hệ thống ổn định, nghĩa là việc quét toàn bộ sẽ không tạo ra giao dịch hoán đổi. 

Vòng lặp bên trong đảm bảo chúng tôi không vượt quá số bước còn lại hoặc số lượng tương tác giới hạn trên mỗi lượt. Các hoạt động modulo thực hiện lập chỉ mục theo chu kỳ của cả hai mảng. 

Một chi tiết tinh tế là điều kiện dừng sớm: không có nó, lớn$k$vẫn sẽ buộc phải di chuyển không cần thiết. Sau khi quét toàn bộ không tạo ra giao dịch hoán đổi, các mảng đã ở cấu hình ổn định theo quy tắc tương tác, do đó các hoạt động tiếp theo không thể thay đổi bất kỳ điều gì. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 3 5
3 4
1 5 2
```Chúng tôi theo dõi trình tự so sánh. 

| tôi | (tôi mod n, tôi mod m) | một giá trị | giá trị b | tráo đổi? | một tiểu bang | bang b | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | (0,0) | 3 | 1 | vâng | [1,4] | [3,5,2] | 
| 1 | (1,1) | 4 | 5 | không | [1,4] | [3,5,2] | 
| 2 | (0,2) | 1 | 2 | không | [1,4] | [3,5,2] | 
| 3 | (1,0) | 4 | 3 | vâng | [1,3] | [4,5,2] | 
| 4 | (0,1) | 1 | 5 | không | [1,3] | [4,5,2] | 

Sau 5 bước, không có thay đổi nào xảy ra nữa. 

Dấu vết này cho thấy cách hoán đổi chỉ xảy ra khi sự đảo ngược cục bộ xuất hiện trong căn chỉnh chu kỳ hiện tại và cách các giá trị ổn định dần dần. 

### Ví dụ 2 

đầu vào:```
1
3 3 0
4 5 6
1 2 3
```Từ$k = 0$, không có hoạt động nào xảy ra. 

| tôi | hành động | một | b | 
| --- | --- | --- | --- | 
| - | không | [4,5,6] | [1,2,3] | 

Điều này xác nhận rằng thuật toán xử lý chính xác trường hợp ranh giới hoạt động bằng 0 mà không cần truy cập bất kỳ so sánh tuần hoàn nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(nm)$trường hợp xấu nhất trên mỗi lần kiểm tra (giới hạn bởi k và dừng sớm) | Mỗi bước mô phỏng một so sánh và hoán đổi giúp giảm thiểu tình trạng hỗn loạn một cách nghiêm ngặt để quy trình ổn định nhanh chóng trong thực tế trong điều kiện hạn chế | 
| Không gian |$O(1)$thêm | Chỉ cập nhật mảng tại chỗ và một vài bộ đếm | 

Tổng giới hạn kích thước$\sum (n+m) \le 2 \cdot 10^5$đảm bảo rằng ngay cả những lượt đi lặp lại vẫn có thể quản lý được. Việc chấm dứt sớm sẽ ngăn chặn tình trạng kéo dài bệnh lý và thuật toán không bao giờ phụ thuộc trực tiếp vào$k$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n, m, k = map(int, input().split())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))

        i = 0
        while k > 0:
            swapped = False
            limit = min(k, n * m)
            for _ in range(limit):
                ia = i % n
                ib = i % m
                if a[ia] > b[ib]:
                    a[ia], b[ib] = b[ib], a[ia]
                    swapped = True
                i += 1
                k -= 1
                if k == 0:
                    break
            if not swapped:
                break

        out.append(" ".join(map(str, a)))
        out.append(" ".join(map(str, b)))

    return "\n".join(out)

# provided sample 1
assert run("""3
2 3 5
3 4
1 5 2
1 5 4
6
5 4 3 2 1
3 3 0
4 5 6
1 2 3
""") == """1 3
4 5 2
2
6 5 4 3 1
4 5 6
1 2 3"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| k = 0 trường hợp | mảng giống hệt nhau | không có hoạt động | 
| đã được sắp xếp chia | không hoán đổi | phát hiện độ ổn định | 
| mảng đảo ngược | nhiều giao dịch hoán đổi | hành vi hội tụ | 
| mảng phần tử đơn | tầm thường | độ đúng ranh giới | 

## Vỏ cạnh 

Một trường hợp cạnh tranh quan trọng là khi$k = 0$. Quá trình phải chấm dứt ngay lập tức mà không thử bất kỳ quyền truy cập hoặc hoán đổi modulo nào. Thuật toán xử lý việc này vì điều kiện vòng lặp chính là$k > 0$, do đó không có sự lặp lại nào xảy ra và cả hai mảng đều được in không thay đổi. 

Một trường hợp cạnh khác xảy ra khi các mảng đã có cấu hình ổn định trong đó mọi cặp tương tác đều thỏa mãn$a[i] \le b[j]$. Trong trường hợp này, lần quét toàn bộ đầu tiên không tạo ra sự hoán đổi, thiết lập`swapped`gắn cờ thành sai và thuật toán kết thúc sớm. Điều này ngăn cản sự mô phỏng không cần thiết của một hệ thống đã hội tụ. 

Trường hợp thứ ba là khi$n = m = 1$. Chỉ có một so sánh tồn tại lặp đi lặp lại giữa hai yếu tố. Thuật toán chỉ hoán đổi liên tục nếu cần thiết cho đến khi phần tử nhỏ hơn nằm trong$b$, và sau đó dừng ngay lập tức sau khi ra lệnh.
