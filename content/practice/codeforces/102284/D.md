---
title: "CF 102284D - \u041a\u0440\u0430\u0441\u0438\u0432\u044b\u0435 \u043c\u0435\u043b\u043e\u0434\u0438\u0438"
description: "Chúng tôi có một loạt các tần số nốt (n). Chúng ta có thể chọn bất kỳ dãy con nào khác trống, do đó các vị trí được chọn phải giữ nguyên thứ tự ban đầu nhưng các vị trí có thể bị bỏ qua."
date: "2026-08-13T08:47:23+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102284
codeforces_index: "D"
codeforces_contest_name: "\u041b\u041a\u0428 2019, \u0418\u044e\u043b\u044c, \u041c\u0438\u043a\u0441 \u0441\u0442\u0430\u0440\u0448\u0435\u0439 \u0438 \u043c\u043b\u0430\u0434\u0448\u0435\u0439 \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434"
rating: 0
weight: 102284
solve_time_s: 300
verified: true
draft: false
---

[CF 102284D - \u041a\u0440\u0430\u0441\u0438\u0432\u044b\u0435 \u043c\u0435\u043b\u043e\u0434\u0438\u0438](https://codeforces.com/problemset/problem/102284/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 5 phút 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một loạt các tần số nốt (n). Chúng ta có thể chọn bất kỳ dãy con nào khác trống, do đó các vị trí được chọn phải giữ nguyên thứ tự ban đầu nhưng các vị trí có thể bị bỏ qua. Dãy con này đẹp khi hiệu tuyệt đối giữa hai tần số được chọn lân cận là số nguyên tố. 

Nhiệm vụ không chỉ đơn thuần là đếm các chuỗi con như vậy. Chúng ta phải sắp xếp tất cả các chuỗi con đẹp theo từ điển theo chuỗi tần số của chúng và xuất ra chuỗi thứ (k). Nếu có thể thu được cùng một chuỗi tần số từ nhiều lựa chọn vị trí khác nhau thì mỗi lần xuất hiện như vậy đều đóng góp riêng vào thứ tự. 

Các ràng buộc làm cho việc liệt kê trực tiếp là không thể. Với (n=1000), có thể có (2^{1000}-1) dãy con không trống, do đó, ngay cả việc tạo ra chúng cũng sẽ vô vọng. Tuyên bố chính thức đưa ra giới hạn thời gian 2 giây và giới hạn bộ nhớ 512 MB. Do đó, mục tiêu hữu ích là xung quanh (O(n^2)), tức là khoảng một triệu thao tác cấp mảng trước khi tính đến quá trình tiền xử lý nguyên tố. 

Giá trị giới hạn của (100000) đủ lớn để việc kiểm tra tính nguyên tố độc lập cho từng cặp sẽ là lãng phí, nhưng đủ nhỏ để sàng lọc. Sự khác biệt lớn nhất có thể có cũng nằm dưới (100000), do đó, một sàng sẽ trả lời mọi truy vấn về khả năng tương thích. 

Có một số trường hợp nguy hiểm có thể âm thầm phá vỡ một giải pháp hợp lý. Đầu tiên là mảng nhỏ nhất có thể. 

Vì```
1 1
5
```dãy con đẹp duy nhất là ((5)), nên đầu ra là```
1
5
```Một giải pháp bao gồm dãy con trống trong quá trình đếm của nó sẽ trả về thứ hạng sai, bởi vì bài toán chỉ xét đến các dãy con khác trống. 

Các giá trị lặp lại tạo ra một cái bẫy khác. Vì```
3 2
7 7 7
```dãy con đẹp duy nhất là ba lần xuất hiện đơn lẻ, vì hiệu bằng 0 không phải là số nguyên tố. Do đó, điều thứ hai là```
1
7
```Việc triển khai bất cẩn coi số 0 là sai phân nguyên tố hợp lệ hoặc chỉ tính các chuỗi tần số riêng biệt thay vì số lần xuất hiện sẽ đưa ra một câu trả lời khác. 

Việc xử lý từ điển các tiền tố trùng lặp cũng rất cần thiết. Coi như```
3 4
1 3 1
```Cả hai (1) đều có thể tạo thành một đơn vị và hiệu giữa (1) và (3) là số nguyên tố. Danh sách thứ tự là 

[ 
(1), (1), (1,3), (1,3,1), (3), (3,1). 
] 

Vì vậy câu trả lời thứ tư là```
3
1 3 1
```Một giải pháp nhóm các giá trị đầu tiên bằng nhau theo lần xuất hiện đầu tiên của chúng sẽ đặt không chính xác ((1,3)) trước đơn vị thứ hai ((1)). 

Cuối cùng, sự khác biệt của một không được chấp nhận. Vì```
2 2
1 2
```không cặp nào có thể được mở rộng, vì vậy câu trả lời là```
1
2
```Hai dãy con đơn là dãy đẹp duy nhất. Điều này nắm bắt các triển khai chỉ kiểm tra xem hai tần số có khác nhau hay không. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Tạo mọi dãy con khác rỗng của mảng, kiểm tra xem mọi sai phân lân cận có phải là số nguyên tố hay không, lưu trữ những dãy đẹp, sắp xếp chúng theo thứ tự từ điển và lấy phần tử thứ (k). Có (2^n-1) dãy con và việc kiểm tra một dãy có thể lấy (O(n)), vì vậy trường hợp xấu nhất là (O(n2^n)). Với (n=1000), điều này vượt xa mọi giới hạn thực tế. 

Lập trình động ngay lập tức cung cấp cho chúng ta một phép đếm nguyên thủy hữu ích. Với mọi vị trí (i), gọi (cách[i]) là số dãy con đẹp có vị trí được chọn đầu tiên chính xác là (i). Singleton chỉ bao gồm (a_i) đóng góp một. Mỗi vị trí sau (j) có chênh lệch nguyên tố có thể là vị trí được chọn tiếp theo, sau đó có (cách[j]) khả năng tiếp tục. Như vậy 

1+ 
\sum_{\substack{j>i\ |a_i-a_j|\text{ là số nguyên tố}}} 
cách[j]. 
] 

Phép lặp này có thể được đánh giá từ phải sang trái trong (O(n^2)). 

Khó khăn còn lại là việc không xếp hạng từ điển. Bạn nên sử dụng trực tiếp (cách[i]) và chọn vị trí bắt đầu, nhưng điều đó là sai khi một số vị trí có cùng tần số. Ví dụ: trong ((1,3,1)), cả hai đơn vị ((1)) đều phải xuất hiện trước ((1,3)). Nhóm thứ tự từ điển theo tần số, không phải theo vị trí được sử dụng để thu được tần số đó. 

Quan sát quan trọng là sau khi chúng ta sửa một tiền tố, mọi lần nhúng tiền tố đó đều kết thúc với cùng tần số, cụ thể là tần số cuối cùng của tiền tố. Chúng ta có thể giữ số lượng phần nhúng kết thúc ở mọi vị trí trong một mảng (trọng lượng). 

Giả sử tiền tố hiện tại đã được cố định và (weight[i]) là số phần nhúng của nó kết thúc ở vị trí (i). Khi đó số lần xuất hiện của tiền tố đó chỉ đơn giản là 

[ 
\sum_i trọng lượng[i]. 
] 

Vì một chuỗi nhỏ hơn về mặt từ điển so với bất kỳ phần mở rộng thích hợp nào của chính nó nên toàn bộ khối này phải được xem xét trước khi xem xét tần số tiếp theo khác. 

Bây giờ hãy xem xét việc mở rộng tiền tố theo giá trị (v). Đối với vị trí (j) với (a_j=v), số lượng nhúng của tiền tố mở rộng kết thúc tại (j) là 

\sum_{\substack{i<j\ |a_i-v|\text{ là số nguyên tố}}} 
trọng lượng [i]. 
] 

Khi đó số dãy con đẹp hoàn chỉnh sử dụng vị trí này làm phần tử mới được chọn là 

[ 
newWeight[j]\cdot cách[j]. 
] 

Chúng ta cần nhóm các số đếm này theo (v), bởi vì tất cả các chuỗi có tần số tiếp theo giống nhau đều thuộc về một khối từ điển. 

Có một sự đơn giản hóa quan trọng. Tất cả khác không (trọng số [i]) thuộc về các vị trí có tần số cuối cùng của tiền tố hiện tại (c). Do đó, khi quét các vị trí từ trái sang phải, tổng cần thiết cho vị trí trong tương lai (j) chỉ là trọng số tích lũy của các vị trí trước đó, với điều kiện (|c-a_j|) là số nguyên tố. Do đó, một lần quét mảng sẽ tính toán số lượng cho mọi tần số tiếp theo có thể xảy ra cùng một lúc. 

Sau khi chọn tần số tiếp theo, quá trình quét tương tự đã tính toán các trọng số mới cho tất cả các vị trí chứa tần số đó. Chúng ta có thể loại bỏ tất cả các vị trí khác và tiếp tục. 

Phương pháp brute-force hoạt động vì mọi dãy con đều được xem xét rõ ràng, nhưng không thành công vì số lượng của chúng là số mũ. Quan sát rằng tất cả các phần nhúng của tiền tố hiện tại có một tần số chung cuối cùng cho phép chúng tôi tổng hợp chúng thành các trọng số vị trí và cập nhật tất cả các khối từ điển trong một lần quét tuyến tính. Kết quả không xếp hạng sẽ mất (O(n)) cho mỗi phần tử đầu ra và có thể có tối đa (n) phần tử. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n2^n)) | (O(n2^n)) nếu tất cả các chuỗi con được lưu trữ | Quá chậm | 
| Tối ưu | (O(n^2)) | (O(n+V)) | Đã chấp nhận | 

Ở đây (V\le 100000) là độ chênh lệch tần số tối đa liên quan đến sàng. 

## Hướng dẫn thuật toán 

1. Xây dựng một sàng Eratosthenes có độ chênh lệch tần số lớn nhất có thể. Sau quá trình tiền xử lý này,`prime[d]`cho chúng ta biết trong thời gian (O(1)) liệu (d) có phải là số nguyên tố hay không. 
2. Tính toán`ways[i]`từ phải sang trái. Ban đầu mọi vị trí đều đóng góp đơn vị của nó, vì vậy`ways[i]`bắt đầu từ một. Đối với mọi vị trí sau (j), hãy thêm`ways[j]`khi độ lệch tần số là số nguyên tố. Các giá trị có thể được giới hạn ở (k), vì số lượng lớn hơn (k) không thể phân biệt được khi lựa chọn thứ hạng. 
3. Trước khi xây dựng bất kỳ tiền tố nào, hãy nhóm tất cả các chuỗi con đẹp theo tần số đầu tiên của chúng. Mỗi vị trí (i) đều đóng góp`ways[i]`tuần tự vào khối có giá trị đầu tiên là`a[i]`. Sắp xếp các tần số riêng biệt và trừ toàn bộ các khối cho đến khi tìm thấy khối chứa thứ hạng (k). 
4. Đặt`weight[i]`đến một cho mọi vị trí có giá trị là tần số đầu tiên được chọn và bằng 0 ở những nơi khác. Tại thời điểm này`weight[i]`có nghĩa chính xác là số lần nhúng của tiền tố hiện tại kết thúc ở vị trí (i). 
5. Đối với tiền tố hiện tại, trước tiên hãy tính`exact = sum(weight)`. Đây là tất cả các lần xuất hiện của tiền tố. Nếu (k\le chính xác), tiền tố là câu trả lời bắt buộc, bởi vì về mặt từ điển, một chuỗi nhỏ hơn mọi phần mở rộng thích hợp của chính nó. 
6. Nếu tiền tố không phải là câu trả lời, hãy trừ`exact`từ (k). Thứ hạng còn lại thuộc về một phần mở rộng thích hợp. 
7. Quét mảng từ trái sang phải. Duy trì`pref`, tổng trọng lượng của các vị trí trước vị trí hiện tại. Đối với mọi vị trí (j), nếu chênh lệch giữa giá trị tiền tố hiện tại và`a[j]`là số nguyên tố thì`pref`chính xác là số lần nhúng của tiền tố hiện tại có thể được mở rộng thông qua (j). Thêm vào`pref * ways[j]`vào khối từ điển thuộc về`a[j]`. 
8. Trong cùng quá trình quét, lưu trữ`pref`là trọng số mới của vị trí (j) bất cứ khi nào quá trình chuyển đổi hợp lệ. Khi giá trị tiếp theo đã chọn (v) được biết, tất cả các vị trí có giá trị không phải là (v) sẽ bị loại bỏ. Các trọng số còn lại mô tả mỗi lần nhúng tiền tố mới. 
9. Lặp lại ba bước trước đó cho đến khi tiền tố hiện tại chứa thứ hạng được yêu cầu. Vì dãy con sử dụng các vị trí tăng dần nên độ dài của nó không bao giờ vượt quá (n). 

### Tại sao nó hoạt động 

Điều bất biến là sau khi xây dựng tiền tố (P),`weight[i]`bằng số lượng nhúng của (P) có vị trí được chọn cuối cùng là (i). Do đó, tổng các trọng số này sẽ tính mọi lần xuất hiện của (P), bao gồm các chuỗi tần số trùng lặp được tạo ra bởi các vị trí khác nhau. 

Đối với bất kỳ vị trí tiếp theo nào có thể có (j), quá trình quét sẽ tính toán chính xác số lượng phần nhúng của (P) có thể đạt tới (j). Nhân cái này với`ways[j]`đếm mọi sự hoàn thành đẹp đẽ bắt đầu bằng sự lựa chọn đó đúng một lần. Nhóm các giá trị này theo`a[j]`đưa ra kích thước chính xác của mỗi khối giá trị tiếp theo từ điển. 

Các khối được kiểm tra theo thứ tự tần số tăng dần, do đó việc bỏ qua toàn bộ khối sẽ giữ nguyên thứ hạng của chuỗi mong muốn. Khi một khối được chọn, khối tương ứng của nó`newWeight`mảng thể hiện chính xác tất cả các phần nhúng bên trong khối đó. Do đó, bất biến được giữ nguyên sau mỗi tần số đã chọn và khi thứ hạng nằm trong khối tiền tố chính xác, tiền tố hiện tại chính xác là chuỗi con được yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve_case(n, k, a):
    max_diff = max(a) - min(a)

    prime = bytearray(b'\x01') * (max_diff + 1)
    if max_diff >= 0:
        prime[0] = 0
    if max_diff >= 1:
        prime[1] = 0

    p = 2
    while p * p <= max_diff:
        if prime[p]:
            start = p * p
            cnt = (max_diff - start) // p + 1
            prime[start:max_diff + 1:p] = b'\x00' * cnt
        p += 1

    cap = k

    ways = [1] * n

    for i in range(n - 1, -1, -1):
        total = 1
        ai = a[i]

        for j in range(i + 1, n):
            if prime[abs(ai - a[j])]:
                total += ways[j]
                if total >= cap:
                    total = cap
                    break

        ways[i] = total

    values = sorted(set(a))

    first_count = {}
    for i, v in enumerate(a):
        old = first_count.get(v, 0)
        cur = old + ways[i]
        first_count[v] = cap if cur >= cap else cur

    for v in values:
        cnt = first_count.get(v, 0)
        if k > cnt:
            k -= cnt
        else:
            first_value = v
            break

    answer = [first_value]

    weight = [0] * n
    for i in range(n):
        if a[i] == first_value:
            weight[i] = 1

    current = first_value

    while True:
        exact = sum(weight)

        if k <= exact:
            break

        k -= exact

        group = {}
        new_weight = [0] * n
        pref = 0

        for j in range(n):
            aj = a[j]

            if pref and prime[abs(current - aj)]:
                new_weight[j] = pref

                add = pref * ways[j]
                old = group.get(aj, 0)
                cur = old + add
                group[aj] = cap if cur >= cap else cur

            if weight[j]:
                pref += weight[j]
                if pref >= cap:
                    pref = cap

        chosen = None

        for v in values:
            cnt = group.get(v, 0)
            if k > cnt:
                k -= cnt
            else:
                chosen = v
                break

        answer.append(chosen)
        current = chosen

        for i in range(n):
            if a[i] == chosen:
                weight[i] = new_weight[i]
            else:
                weight[i] = 0

    return answer

def main():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))

    answer = solve_case(n, k, a)

    print(len(answer))
    print(*answer)

if __name__ == "__main__":
    main()
```Sàng chỉ được xây dựng dựa trên sự khác biệt lớn nhất có thể thực sự xảy ra. các`bytearray`cách biểu diễn giữ cho bảng nguyên tố nhỏ gọn và làm cho sàng dựa trên việc cắt lát đủ nhanh cho Python. 

các`ways`sự lặp lại tuân theo bước lập trình động từ phải sang trái. Phá vỡ khi đạt đến số đếm`k`là an toàn vì tất cả các bổ sung sau này đều không âm, do đó giá trị chính xác không còn quan trọng đối với việc hủy xếp hạng. 

Việc tính toán tần số đầu tiên tách biệt với việc xây dựng lại chính vì không có tần số trước đó ở gốc. Mỗi vị trí có thể độc lập là vị trí được chọn đầu tiên, vì vậy khối giá trị (v) là tổng của`ways[i]`trên tất cả các vị trí chứa (v). 

Khi giá trị đầu tiên được cố định,`weight[i]`thể hiện phần nhúng của tiền tố hiện tại hoàn chỉnh, không chỉ đơn thuần là các vị trí mà tiền tố đó có thể xuất hiện. Sự khác biệt này là yếu tố xử lý chính xác các chuỗi con trùng lặp. 

Bên trong bản quét tái thiết,`pref`chỉ được cập nhật sau khi xử lý vị trí (j). Dãy con phải sử dụng vị trí sớm hơn trước (j), do đó bao gồm`weight[j]`quá sớm sẽ gây ra sự tự chuyển đổi không hợp lệ. Sự khác biệt được kiểm tra trước khi trọng lượng mới được thêm vào vì lý do tương tự. 

biểu hiện`pref * ways[j]`đếm tất cả các lần hoàn thành thông qua (j).`ways[j]`đã bao gồm tùy chọn dừng ở (j), điều này là cần thiết vì bản thân tiền tố hiện tại phải xuất hiện trước phần mở rộng của nó theo thứ tự từ điển. 

Số nguyên Python không bị tràn. Số lượng giới hạn tại`k`Ngoài ra, ngăn chặn sự tăng trưởng không cần thiết và cho phép thuật toán bỏ qua sự khác biệt giữa số lượng mà cả hai đều đủ lớn để chứa thứ hạng được yêu cầu. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Mảng là (1,3,6,1). Số dãy con đẹp bắt đầu ở mỗi vị trí là 

[ 
cách=[7,4,2,1]. 
] 

Do đó, các khối giá trị đầu tiên chứa 9 chuỗi bắt đầu bằng (1), 4 chuỗi bắt đầu bằng (3) và 1 chuỗi bắt đầu bằng (6). 

| Sân khấu | Tiền tố hiện tại | Số tiền tố chính xác | Xếp hạng trước khi quyết định | Quyết định | 
| --- | --- | --- | --- | --- | 
| Gốc | trống | không áp dụng | 2 | chọn 1, khối của nó có 9 | 
| 1 | (1) | 2 | 2 | dừng lại | 

Hai lần xuất hiện của tiền tố ((1)) đều được tính bằng`weight`: vị trí 0 có trọng số 1 và vị trí 3 có trọng số 1. Vì thứ hạng được yêu cầu chính xác là 2 nên chính tiền tố là câu trả lời. 

Kết quả là```
1
1
```Ví dụ này giải thích tại sao các phần nhúng trùng lặp phải được tính trước khi xem xét các phần mở rộng dài hơn. 

### Mẫu 2 

Số lượng khối giá trị đầu tiên lại là (9,4,1) cho các giá trị (1,3,6). 

| Sân khấu | Tiền tố hiện tại | Số lượng chính xác | Xếp hạng bước vào giai đoạn | Khối mở rộng | Giá trị được chọn | 
| --- | --- | --- | --- | --- | --- | 
| Gốc | trống | không áp dụng | 6 | (1:9,\ 3:4,\ 6:1) | 1 | 
| 1 | (1) | 2 | 6 | (3:4,\ 6:2) | 3 | 
| 2 | (1,3) | 1 | 3 | (1:1,\ 6:2) | 6 | 
| 3 | (1,3,6) | 1 | 2 | (1:1) | 1 | 
| 4 | (1,3,6,1) | 1 | 1 | không cần thiết | dừng lại | 

Sau khi chọn cái đầu tiên (1), hai lần xuất hiện đơn lẻ chiếm hai hạng đầu tiên trong khối của nó, do đó hạng còn lại là (6-2=4). Khối giá trị tiếp theo cho (3) có kích thước 4, vì vậy (3) được chọn. 

Đối với tiền tố ((1,3)), tiền tố đó chiếm một vị trí trong thứ tự. Thứ hạng còn lại của nó là 3 và các khối mở rộng có kích thước 1 cho giá trị tiếp theo (1) và 2 cho giá trị tiếp theo (6). Do đó, hạng thứ ba bên trong cây con này là ((1,3,6)). 

Đối số tương tự chọn cuối cùng (1), đưa ra```
4
1 3 6 1
```Dấu vết thể hiện tính bất biến trung tâm: thứ hạng luôn được đo bên trong cây con được biểu thị bằng tiền tố hiện tại, trong khi`weight`giữ tất cả các phần nhúng có thể có của nó cùng một lúc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n^2 + V\log\log V)) | DP lấy (O(n^2)) và tái cấu trúc quét mảng phần tử (n) nhiều nhất là (n) lần. Sàng xử lý sự khác biệt lên đến (V\le100000). | 
| Không gian | (O(n+V)) | Các mảng chính có kích thước (O(n)), trong khi bảng nguyên tố có kích thước (O(V)). | 

Thuật ngữ chiếm ưu thế là (O(n^2)), khoảng một triệu thao tác cấp vị trí cho (n=1000). Đầu vào hợp lệ cũng đảm bảo rằng (k) không vượt quá số lượng chuỗi con có sẵn, tối đa là (2^{1000}-1), do đó số lượng liên quan chỉ yêu cầu khoảng 1000 bit mặc dù giới hạn trên cú pháp lớn hơn nhiều đối với (k). 

## Trường hợp thử nghiệm```python
import sys
import io

# Paste the solve_case function from the solution above before running these tests.

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    try:
        n, k = map(int, input().split())
        a = list(map(int, input().split()))
        ans = solve_case(n, k, a)
        return str(len(ans)) + "\n" + " ".join(map(str, ans)) + "\n"
    finally:
        sys.stdin = old_stdin

# Provided samples
assert run("""4 2
1 3 6 1
""") == """1
1
""", "sample 1"

assert run("""4 6
1 3 6 1
""") == """4
1 3 6 1
""", "sample 2"

# Minimum-size input
assert run("""1 1
5
""") == """1
5
""", "minimum size"

# Maximum-size input, all equal values.
# No pair can be adjacent because the difference is zero.
max_input = "1000 1000\n" + " ".join(["7"] * 1000) + "\n"
assert run(max_input) == """1
7
""", "maximum size and all equal"

# Difference 1 is not prime.
assert run("""2 2
1 2
""") == """1
2
""", "non-prime boundary"

# Duplicate prefixes and exact-prefix ordering.
assert run("""3 4
1 3 1
""") == """3
1 3 1
""", "duplicate prefix ordering"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 5`|`1 / 5`| Mảng nhỏ nhất có thể và loại trừ dãy con trống | 
|`1000 1000 / 7 ... 7`|`1 / 7`| Tối đa (n), các giá trị bằng nhau lặp lại và số 0 không phải là số nguyên tố | 
|`2 2 / 1 2`|`1 / 2`| Sự khác biệt phải bị từ chối | 
|`3 4 / 1 3 1`|`3 / 1 3 1`| Các lần xuất hiện đơn lẻ trùng lặp và thứ tự tiền tố trước khi mở rộng | 

## Vỏ cạnh 

Đối với đầu vào một phần tử```
1 1
5
```khối ban đầu cho giá trị (5) có kích thước bằng một vì`ways[0] = 1`. Giá trị đầu tiên được chọn,`weight[0] = 1`và số tiền tố chính xác là một. Vì (k=1), quá trình tái thiết dừng ngay lập tức và xuất ra ((5)). Không bao giờ có bất kỳ nỗ lực chuyển tiếp nào, do đó ranh giới ở cuối mảng được xử lý một cách tự nhiên. 

Vì```
3 2
7 7 7
```sàng đánh dấu số 0 là không nguyên tố. Do đó`ways[i] = 1`ở mọi vị trí. Khối giá trị đầu tiên cho (7) có kích thước ba, bởi vì mỗi vị trí cung cấp một chuỗi con đơn lẻ riêng biệt. Sau khi chọn (7), mảng trọng số chứa ba đơn vị. Do đó, số tiền tố chính xác là ba, do đó xếp hạng hai điểm dừng ở đơn vị. Không có sự chuyển đổi nào có chênh lệch bằng 0 được chấp nhận. 

Vì```
3 4
1 3 1
```khối giá trị đầu tiên cho (1) chứa bốn chuỗi con: hai lần xuất hiện đơn lẻ, ((1,3)) và ((1,3,1)). Do đó hạng bốn sẽ đi vào cây con (1). Tiền tố hiện tại có số đếm chính xác là hai, tương ứng với hai vị trí chứa (1), vì vậy hai thứ hạng đó bị bỏ qua. Giá trị tiếp theo duy nhất là (3) và khối mở rộng của nó chứa hai chuỗi, ((1,3)) và ((1,3,1)). Xếp hạng hai bên trong khối đó chọn (3), sau đó tiền tố chính xác ((1,3)) tiêu thụ một hạng và cuối cùng (1) tiêu thụ hạng còn lại. Đầu ra là ((1,3,1)). Đây chính xác là trường hợp chỉ theo dõi vị trí khớp sớm nhất sẽ không thành công. 

Vì```
2 2
1 2
```sự khác biệt là một, mà sàng đánh dấu là hỗn hợp. Mỗi`ways`giá trị là một, vì vậy các khối ban đầu là (1:1) và (2:1). Xếp hạng hai bỏ qua toàn bộ khối bắt đầu bằng (1) và chọn (2). Singleton kết quả có số đếm chính xác là một, do đó thuật toán kết thúc bằng ((2)). Điều này xác nhận rằng thử nghiệm cơ bản được áp dụng cho chênh lệch tuyệt đối thực tế thay vì chỉ kiểm tra xem các tần số có khác biệt hay không.
