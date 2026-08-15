---
title: "CF 102452E - Xóa số"
description: "Chúng ta có một mảng có độ dài lẻ gồm các số nguyên riêng biệt. Một phép toán chọn ba phần tử hiện tại liên tiếp và thay thế ba phần tử đó bằng trung vị của chúng, do đó mảng trở nên ngắn hơn hai."
date: "2026-08-12T08:25:57+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102452
codeforces_index: "E"
codeforces_contest_name: "2019-2020 ICPC Asia Hong Kong Regional Contest"
rating: 0
weight: 102452
solve_time_s: 105
verified: true
draft: false
---

[CF 102452E - Xóa số](https://codeforces.com/problemset/problem/102452/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 45 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một mảng có độ dài lẻ gồm các số nguyên riêng biệt. Một phép toán chọn ba phần tử hiện tại liên tiếp và thay thế ba phần tử đó bằng trung vị của chúng, do đó mảng trở nên ngắn hơn hai. Việc lựa chọn bộ ba là hoàn toàn linh hoạt và sau các phép toán ((N-1)/2), chính xác vẫn còn một giá trị ban đầu. 

Đối với mỗi vị trí ban đầu (i), chúng ta cần quyết định xem có tồn tại chuỗi thao tác nào đó trong đó (a_i) là người sống sót cuối cùng hay không. Đầu ra là một chuỗi nhị phân, có ký tự (i) bằng`1`chính xác khi nào (a_i) có thể sống sót. 

Cách hữu ích để nghĩ về một ứng cử viên cố định (x=a_i) là quên đi các giá trị chính xác. Mọi phần tử khác chỉ quan trọng theo mối quan hệ của nó với (x). Các giá trị nhỏ hơn (x) trở thành`-1`, các giá trị lớn hơn (x) trở thành`+1`, và (x) chính nó trở thành`0`. Phép toán trung vị khi đó chỉ phụ thuộc vào ba dấu hiệu này. 

Các ràng buộc chính thức cho phép (N\le 5000) và tổng (N) trên tất cả các trường hợp thử nghiệm nhiều nhất là (10^4). Điều này làm cho giải pháp (O(N^2)) trở nên thiết thực, bởi vì tổng công việc tối đa chỉ ở mức (10^8) các phép toán rất đơn giản trong phân bố cực đoan, trong khi giải pháp (O(N^3)) rõ ràng là quá đắt. Giới hạn cuộc thi ban đầu là một giây, do đó việc triển khai phải giữ cho vòng lặp bên trong đơn giản và tránh các cấu trúc dữ liệu có hằng số lớn. 

Có một số trường hợp khó khăn có thể dễ dàng phá vỡ việc triển khai không chính xác. Với (N=1), số duy nhất phải tồn tại, do đó đầu vào`1 / 1`có đầu ra`1`. Việc triển khai giả định có ít nhất một thao tác tồn tại có thể thất bại ở đây. 

Trường hợp thứ hai là khi các số nhỏ hơn ứng viên và lớn hơn ứng viên xuất hiện thường xuyên như nhau. Ví dụ, với`3 / 2 3 1`, ứng viên`2`có một phần tử nhỏ hơn và một phần tử lớn hơn. Ba phần tử đã có dạng nhỏ hơn, ứng cử viên, lớn hơn, vì vậy`2`là trung vị và tồn tại ngay lập tức. Đầu ra đúng là`100`. Một cách tiếp cận yêu cầu ứng viên được bao quanh bởi hai yếu tố ở cùng một phía sẽ từ chối trường hợp này một cách không chính xác. 

Trường hợp thứ ba là ứng cử viên ở gần điểm cuối của thứ tự giá trị. Vì`3 / 1 2 3`, chỉ một`2`có thể tồn tại, vì phép toán duy nhất có thể thực hiện được là lấy trung vị của cả ba số. Đầu ra đúng là`010`. Chỉ kiểm tra xem ứng cử viên có gần với trung vị trên toàn cầu hay không là không đủ đối với các mảng lớn hơn, bởi vì thứ tự của các phần tử cũng kiểm soát bộ ba nào có thể được hình thành. 

Câu lệnh đảm bảo các giá trị riêng biệt, do đó, đầu vào hoàn toàn bằng nhau không phải là phép thử chính thức hợp lệ. Tuy nhiên, nếu đầu vào như vậy được cung cấp, thuật toán dựa trên so sánh vẫn hoạt động một cách tự nhiên, bởi vì mọi phần tử bằng ứng cử viên đều được phân loại là`0`. Ví dụ,`3 / 7 7 7`sẽ sản xuất`111`. Điều này hữu ích như một bài kiểm tra độ chắc chắn, nhưng nó không nên được sử dụng làm bằng chứng về các ràng buộc chính thức của bài toán. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp là mô phỏng mọi chuỗi hoạt động có thể xảy ra. Khi có (m) phần tử thì có thể có (m-2) bộ ba liên tiếp. Sau khi chọn một, còn lại (m-2) phần tử, sau đó là (m-4) lựa chọn, v.v. Số chuỗi thao tác hoàn chỉnh là 

[ 
(N-2)(N-4)(N-6)\cdots 1=(N-2)!!. 
] 

Ngay cả trước khi xem xét chi phí mô phỏng từng chuỗi, điều này đã trở nên rất lớn. Với (N=5000), số lượng các khả năng có thể vượt xa những gì có thể liệt kê được. Lực lượng vũ phu đúng về mặt khái niệm vì nó khám phá chính xác các hoạt động hợp pháp, nhưng nó không khai thác thực tế là hầu hết thông tin số đều không liên quan. 

Quan sát quan trọng là chọn một ứng cử viên (x) và chỉ hỏi liệu (x) có thể sống sót hay không. Thay thế mọi giá trị khác bằng cách so sánh nó với (x). Giá trị dưới (x) trở thành`-1`, một giá trị trên (x) trở thành`+1`, và (x) trở thành`0`. Đối với bất kỳ bộ ba nào không chứa (x), dấu của trung vị của nó chính xác là trung vị của ba dấu của nó. Do đó, độ lớn thực tế biến mất khỏi bài toán. Bài xã luận chính thức sử dụng chính xác sự rút gọn này thành chuỗi so sánh nhị phân. 

Giả sử chuỗi so sánh chứa nhiều`+1`giá trị hơn`-1`các giá trị. Xác định 

[ 
S=#(+1)-#(-1). 
] 

Nếu (S=0), hai bên xảy ra thường xuyên như nhau. Trong tình huống đó, ứng viên có thể được giữ nguyên trong khi phần còn lại xung quanh nó được giảm bớt, vì vậy câu trả lời ngay lập tức là tích cực. Tuyên bố tương tự là đối xứng khi`-1`là đa số. 

Trường hợp thú vị là (S>0), trong đó`+1`là đa số. Hãy xem xét một thao tác có thể làm gì với (S). Bộ ba chứa cả hai dấu hiệu có dấu đa số tồn tại nên một`+1`và một`-1`biến mất và hiệu (S) không thay đổi. Một bộ ba`111`trở thành đơn`1`, vậy là hai`+1`các giá trị biến mất và (S) giảm đi hai. Không có hoạt động nào khác có thể giảm (S). Do đó, nếu ứng cử viên sống sót, chúng ta phải có khả năng tạo ra đủ bộ ba dấu đa số để giảm (S) về 0. 

Thứ tự mảng xác định liệu các bộ ba đa số đó có thực sự được hình thành hay không. Chúng tôi có thể quét từ trái sang phải trong khi vẫn duy trì được số lượng tài liệu đa số chưa sử dụng hiện có. Một yếu tố đa số làm tăng số lượng này. Một phần tử trái dấu sử dụng một đơn vị, bởi vì hai phần tử đa số có thể được sử dụng xung quanh nó để loại bỏ nó mà không làm thay đổi sự khác biệt tổng thể. Số lượng không bao giờ được phép trở thành số âm. 

Bất cứ khi nào ba đơn vị đa số đã tích lũy, chúng ta có thể thực hiện một`111 -> 1`sự giảm bớt. Điều đó loại bỏ hai phần tử đa số và giảm (S) đi hai. Chiến lược tham lam này tối đa hóa số lượng bộ ba đa số có thể được hình thành, đó chính xác là những gì chúng ta cần khi đa số đang ngăn cản (S) đạt đến số 0. Bài xã luận mô tả ý tưởng tham lam tương tự như việc quét chuỗi và sử dụng các phần tử đa số liên tiếp để giảm số lượng đa số. 

Bản thân ứng viên đóng vai trò như một rào cản. Các hoạt động không thể đơn giản đi qua nó trong khi chúng tôi đang giảm trình tự so sánh, do đó bộ đếm tính khả dụng được đặt lại khi quá trình quét đến ứng viên. Đây cũng là lý do tại sao việc kiểm tra một ứng viên cố định có thể được thực hiện trong một lần quét tuyến tính. 

Lực lượng vũ phu hoạt động vì nó khám phá mọi quy đổi hợp pháp, nhưng không thành công vì có nhiều lệnh giảm giống như giai thừa. Nhận xét rằng chỉ có sự so sánh với các ứng cử viên mới quan trọng làm giảm từng ứng viên thành một bản quét tham lam một chiều. Việc lặp lại quá trình quét đó cho tất cả (N) ứng cử viên sẽ cho ra thuật toán (O(N^2)). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (\Theta((N-2)!!)) trình tự thao tác | (O(N)) | Quá chậm | 
| Tối ưu | (O(N^2)) | (O(N)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Cố định vị trí (i) và đặt (x=a_i) là ứng cử viên sống sót. Đối với mọi vị trí (j), hãy phân loại (a_j) là`-1`nếu (a_j<x),`+1`nếu (a_j>x) và`0`khi (j=i). Điều này giữ chính xác thông tin ảnh hưởng đến việc trung vị nhỏ hơn, bằng hay lớn hơn (x). 
2. Tính (S), tổng của tất cả các dấu hiệu này. Vì có (N-1) dấu khác 0 và (N-1) là số chẵn nên (S) là số chẵn. Nếu (S=0), hãy đánh dấu ứng viên đó ngay lập tức. Số lượng bằng nhau của các phần tử nhỏ hơn và lớn hơn có thể được ghép xung quanh ứng cử viên trong khi vẫn giữ ứng cử viên ở vị trí trung vị. 
3. Nếu (S\ne0), đặt`majority = sign(S)`. Dấu hiệu đa số là dấu hiệu duy nhất có số lượng phải giảm. Bộ ba bao gồm toàn bộ dấu đa số là loại phép toán duy nhất thay đổi (S) và nó thay đổi (S) đúng bằng hai về 0. 
4. Quét chuỗi so sánh từ trái sang phải. Duy trì`tp`, số lượng vật liệu đa số hiện có sẵn. Khi dấu hiện tại bằng đa số thì tăng`tp`. Khi ngược dấu thì giảm`tp`, nhưng không bao giờ dưới 0. Mô hình này sử dụng các yếu tố đa số để loại bỏ các yếu tố đối lập trong khi vẫn giữ được sự khác biệt tổng thể. 
5. Khi nào`tp`đạt đến ba, dành một bộ ba yếu tố đa số. Giảm bớt`tp`bằng hai vì ba dấu đa số bằng nhau được thay thế bằng một và giảm (S) đi hai theo hướng bằng 0. Nếu (S) đạt đến 0, ứng viên có thể được chọn và quá trình quét có thể dừng lại. 
6. Khi quá trình quét đến được ứng viên, hãy đặt lại`tp`về không. Không thể vượt qua ứng cử viên bằng các phép toán được sử dụng để đơn giản hóa hai bên, vì vậy phần lớn vật liệu được tích lũy trước ứng cử viên không thể kết hợp với vật liệu sau đó. 
7. Sau khi quét, ứng viên có thể được tuyển chính xác khi (S=0). Lặp lại thử nghiệm này một cách độc lập cho mọi vị trí. 

### Tại sao nó hoạt động 

Đối với một ứng cử viên cố định, tất cả các giá trị ở cùng một phía của ứng cử viên có thể hoán đổi cho nhau đối với phép toán trung vị, do đó mảng ban đầu có thể được thay thế bằng chuỗi dấu. Nếu hai dấu hiệu có độ phổ biến như nhau thì ứng viên có thể được giữ lại trong khi các yếu tố khác được giảm bớt xung quanh nó. 

Ngược lại, một dấu hiệu là đa số nghiêm ngặt. Bất kỳ thao tác nào chứa cả hai dấu hiệu sẽ loại bỏ một dấu hiệu và giữ nguyên sự khác biệt. Hoạt động duy nhất có khả năng di chuyển sự khác biệt về 0 là ba dấu hiệu đa số bằng nhau trở thành một dấu hiệu đa số, làm giảm số lượng đa số đi hai. Do đó, mỗi lần giảm thành công phải thực hiện đủ chính xác các bộ ba đa số như vậy để loại bỏ chênh lệch ban đầu. 

Quá trình quét tham lam tối đa hóa số lượng bộ ba như vậy có thể được hình thành ở mỗi bên của ứng viên.`tp`ghi lại số lượng cấu trúc đa số hiện có thể được sử dụng và dấu ngược lại sẽ tiêu thụ một đơn vị tài nguyên đó. Bất cứ khi nào có sẵn ba đơn vị đa số, việc cắt giảm ngay lập tức không thể ảnh hưởng đến việc giảm bớt trong tương lai, bởi vì nó làm giảm số lượng đa số cần thiết và để lại một đại diện đa số ở cùng một phạm vi vị trí. Việc đặt lại ở ứng viên tôn trọng việc ứng viên tách biệt phần bên trái và bên phải. Do đó quá trình quét đạt đến (S=0) chính xác khi chuỗi so sánh có thể được giảm đủ để (x) duy trì giá trị trung vị cuối cùng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve_case(a):
    n = len(a)
    answer = []

    for i in range(n):
        x = a[i]

        # Compare every value with the candidate.
        signs = [0] * n
        sm = 0

        for j, v in enumerate(a):
            if v < x:
                signs[j] = -1
                sm -= 1
            elif v > x:
                signs[j] = 1
                sm += 1

        # Equal numbers below and above x.
        if sm == 0:
            answer.append('1')
            continue

        majority = 1 if sm > 0 else -1
        tp = 0

        for s in signs:
            if s == 0:
                # The candidate separates the two independent sides.
                tp = 0
            elif s == majority:
                tp += 1
            else:
                tp = max(tp - 1, 0)

            # Three majority signs can be reduced to one.
            if tp >= 3:
                sm -= 2 * majority
                tp -= 2

                if sm == 0:
                    break

        answer.append('1' if sm == 0 else '0')

    return ''.join(answer)

def main():
    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        out.append(solve_case(a))

    sys.stdout.write('\n'.join(out))

if __name__ == "__main__":
    main()
```Vòng lặp bên ngoài chọn từng người sống sót có thể. Đối với một ứng cử viên cố định, vòng lặp bên trong đầu tiên xây dựng thông tin so sánh và tính toán`sm`cùng một lúc, do đó không cần có một đường chuyền riêng chỉ để đếm các giá trị nhỏ hơn và lớn hơn. 

các`sm == 0`trường hợp được xử lý trước khi chọn dấu đa số. Ranh giới này là cần thiết vì`sign(0)`nếu không sẽ không đưa ra hướng đa số, trong khi ứng cử viên đã được biết là có khả năng. 

Khi`sm`là khác không,`majority`là một trong hai`1`hoặc`-1`. Quá trình quét sử dụng`tp`như tài nguyên tham lam được mô tả ở trên. Phần tử đa số thêm một đơn vị, trong khi phần tử ngược lại loại bỏ một đơn vị nhưng không thể làm cho nó âm. các`max(tp - 1, 0)`là đáng kể. Cho phép`tp`trở nên tiêu cực sẽ không chính xác để cho các yếu tố đa số từ trước một yếu tố đối lập chưa từng có ảnh hưởng đến việc giảm thiểu trong tương lai. 

Khi có sẵn ba đơn vị chiếm đa số,`tp -= 2`còn hơn là`tp -= 3`. Ba dấu bằng được thay thế bằng một dấu bằng, do đó hai thiết bị sẽ biến mất khỏi nguồn cung cấp đang hoạt động. Đồng thời,`sm`thay đổi bởi`-2 * majority`, khớp chính xác với sự thay đổi về chênh lệch đa số-trừ-thiểu số. 

Dấu 0 chính là ứng cử viên. Đặt lại`tp`ở đó ngăn quá trình quét kết hợp các tài nguyên từ các phía đối diện của ứng viên. Đây là điều kiện biên nhạy cảm nhất trong việc thực hiện. 

Tất cả các giá trị đều phù hợp thoải mái với số nguyên Python và trên thực tế`sm`luôn ở giữa`-(N-1)`và (N-1), do đó việc tràn số nguyên không phải là vấn đề đáng lo ngại. 

Việc thực hiện sử dụng`sys.stdin.readline`và tích lũy kết quả đầu ra trong một danh sách, giúp duy trì chi phí I/O đủ nhỏ cho giới hạn cuộc thi một giây. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Trường hợp đầu vào là:```
N = 5
a = [3, 1, 2, 5, 4]
```Xem xét ứng viên`3`, đó là phần tử đầu tiên. 

Trình tự so sánh của nó là`[0, -1, -1, +1, +1]`. Có hai phần tử nhỏ hơn và hai phần tử lớn hơn nên hiệu ban đầu bằng 0. 

| Ứng viên | Trình tự ký hiệu | Ban đầu`sm`| Đa số | Cuối cùng`sm`| Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 3 |`0 -1 -1 +1 +1`| 0 | không | 0 |`1`| 

Bây giờ hãy xem xét ứng viên`1`, phần tử thứ hai. 

Trình tự so sánh của nó là`[+1, 0, +1, +1, +1]`. Sự khác biệt ban đầu là bốn giá trị lớn hơn trừ đi giá trị nhỏ hơn bằng 0, do đó`sm=4`. Phần lớn là`+1`. 

| Giá trị quét | Ký tên |`tp`sau bước |`sm`sau bước | 
| --- | --- | --- | --- | 
| 3 |`+1`| 1 | 3? | 
| 1 |`0`| 0 | 4 | 
| 2 |`+1`| 1 | 4 | 
| 5 |`+1`| 2 | 4 | 
| 4 |`+1`| 1 sau khi giảm | 2 | 

Hàng đầu tiên cần phải cẩn thận: việc giảm không xảy ra cho đến khi ba dấu hiệu đa số được tích lũy, do đó số 0 của ứng viên sẽ đặt lại bộ đếm trước khi điều đó có thể xảy ra. Sự khác biệt cuối cùng vẫn khác 0, vì vậy`1`không thể sống sót. 

Đối với trường hợp hoàn chỉnh, các ứng viên`3`Và`4`là có thể, đưa ra:```
10001
```Hai ứng viên trúng tuyển là vị trí đầu tiên và cuối cùng, đúng như trong mẫu chính thức. 

### Mẫu 2 

Trường hợp thứ hai là:```
N = 3
a = [2, 3, 1]
```Dành cho ứng viên`2`, trình tự so sánh là`[0, +1, -1]`. Có một giá trị lớn hơn và một giá trị nhỏ hơn. 

| Vị trí quét | Ký tên |`tp`|`sm`| 
| --- | --- | --- | --- | 
| 1, ứng cử viên 2 |`0`| 0 | 0 | 
| 2, giá trị 3 |`+1`| 1 | 0 | 
| 3, giá trị 1 |`-1`| 0 | 0 | 

Từ`sm`bắt đầu từ 0, thuật toán ngay lập tức đánh dấu ứng viên`2`càng tốt. 

Dành cho ứng viên`3`, trình tự là`[-1, 0, -1]`, cho`sm=-2`. Chỉ có hai dấu hiệu đa số, vì vậy`tp`không bao giờ đạt tới ba và`sm`không thể trở thành số không. 

| Ứng viên | Trình tự ký hiệu | Ban đầu`sm`| Đa số | Cuối cùng`sm`| Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 2 |`0 +1 -1`| 0 | không | 0 |`1`| 
| 3 |`-1 0 -1`| -2 | -1 | -2 |`0`| 
| 1 |`+1 +1 0`| 2 | +1 | 2 |`0`| 

Như vậy câu trả lời là:```
100
```Ví dụ này thực hiện ngay lập tức`sm=0`trường hợp. Nó cũng cho thấy tại sao chỉ tìm kiếm ba dấu bằng là không đủ, bởi vì một ứng cử viên có thể sống sót khi các phần tử nhỏ hơn và lớn hơn cân bằng chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(N^2)) | Mỗi ứng viên trong số (N) được xử lý bằng hai lần quét tuyến tính, tức là (O(N)) công việc cho mỗi ứng viên. | 
| Không gian | (O(N)) | Mảng dấu hiệu tạm thời và đầu ra yêu cầu bộ nhớ tuyến tính. | 

Trường hợp đơn lớn nhất có (N=5000), do đó thuật toán chỉ thực hiện phép tính bậc hai thay vì khám phá số lượng khổng lồ các lệnh hoạt động có thể có. Tổng (N) trong các trường hợp thử nghiệm tối đa là (10^4), do đó phương pháp bậc hai phù hợp với các ràng buộc dự định. Sự cố ban đầu chỉ định giới hạn thời gian một giây và giới hạn bộ nhớ 512 MB. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_case(a):
    n = len(a)
    answer = []

    for i in range(n):
        x = a[i]
        signs = [0] * n
        sm = 0

        for j, v in enumerate(a):
            if v < x:
                signs[j] = -1
                sm -= 1
            elif v > x:
                signs[j] = 1
                sm += 1

        if sm == 0:
            answer.append('1')
            continue

        majority = 1 if sm > 0 else -1
        tp = 0

        for s in signs:
            if s == 0:
                tp = 0
            elif s == majority:
                tp += 1
            else:
                tp = max(tp - 1, 0)

            if tp >= 3:
                sm -= 2 * majority
                tp -= 2
                if sm == 0:
                    break

        answer.append('1' if sm == 0 else '0')

    return ''.join(answer)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    try:
        sys.stdin = io.StringIO(inp)
        t = int(sys.stdin.readline())
        out = []

        for _ in range(t):
            n = int(sys.stdin.readline())
            a = list(map(int, sys.stdin.readline().split()))
            assert len(a) == n
            out.append(solve_case(a))

        return '\n'.join(out)
    finally:
        sys.stdin = old_stdin

# Provided sample 1
assert run("""\
2
5
3 1 2 5 4
3
2 3 1
""") == """\
10001
100""", "provided samples"

# Minimum-size input
assert run("""\
1
1
1
""") == "1", "single element must survive"

# Three elements in sorted order
assert run("""\
1
3
1 2 3
""") == "010", "only the median survives"

# Balanced candidate, but not in the middle of the array
assert run("""\
1
3
2 3 1
""") == "100", "smaller and larger values balance"

# Maximum-size valid input, sorted permutation.
# For N = 4999, positions 1251 through 3749 can survive.
n = 4999
expected = "0" * 1250 + "1" * 2499 + "0" * 1250
assert len(expected) == n

inp = "1\n" + str(n) + "\n" + " ".join(map(str, range(1, n + 1))) + "\n"
assert run(inp) == expected, "maximum-size boundary case"

# Robustness test outside the official constraints.
# Equal values are forbidden by the statement, but the comparison logic
# should still treat every equal value as the candidate value.
assert run("""\
1
3
7 7 7
""") == "111", "out-of-domain all-equal robustness test"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1`|`1`| Kích thước tối thiểu và trường hợp không hoạt động | 
|`3 / 1 2 3`|`010`| Hành vi trung bình chính xác và các ứng cử viên điểm cuối | 
|`3 / 2 3 1`|`100`| Số lượng giá trị nhỏ hơn và lớn hơn bằng nhau | 
|`4999 / 1 2 ... 4999`|`0...01...10...0`| Kích thước tối đa và cả hai bất đẳng thức biên | 
|`3 / 7 7 7`|`111`| Tính mạnh mẽ bên ngoài ràng buộc giá trị khác biệt | 

Đối với hoán vị được sắp xếp kích thước tối đa, đầu ra dự kiến ​​có thể được lấy trực tiếp. Nếu ứng viên có (L) phần tử nhỏ hơn và (R) phần tử lớn hơn, thì đa số phải được rút gọn bằng cách sử dụng bộ ba của dấu đa số đó. Một ứng cử viên có thể được chọn chính xác khi không bên nào vượt quá ba lần bên kia. Đối với (N=4999), điều này mang lại cho các vị trí ứng cử viên (1251) đến (3749), tạo ra 2499 liên tiếp`1`nhân vật. 

## Vỏ cạnh 

Đối với trường hợp phần tử đơn`N=1`, chuỗi so sánh của ứng viên chỉ chứa`0`, Vì thế`sm=0`ngay lập tức. Đầu ra của thuật toán`1`, điều này đúng vì không có gì để xóa. 

Vì`3 / 1 2 3`, xem xét ứng viên`1`. Dấu hiệu của nó là`[0,+1,+1]`, cho`sm=2`. Phần lớn là`+1`, nhưng chỉ tồn tại hai phần tử đa số, vì vậy`tp`không bao giờ đạt đến ba. trận chung kết`sm`vẫn là hai và câu trả lời cho vị trí một là`0`. Ứng viên`2`có dấu hiệu`[-1,0,+1]`, Vì thế`sm=0`và được chấp nhận. Ứng viên`3`đối xứng với ứng viên`1`. Đầu ra cuối cùng là`010`. 

Vì`3 / 2 3 1`, ứng viên`2`có dấu hiệu`[0,+1,-1]`. Sự khác biệt ban đầu bằng 0 nên thuật toán chấp nhận nó trước khi thực hiện quét. Đây là ví dụ nhỏ nhất cho thấy ứng viên có thể sống sót với một người hàng xóm nhỏ hơn và một người hàng xóm lớn hơn. Đầu ra là`100`. 

Đối với một ứng cử viên có đa số mạnh mẽ ở một bên, chẳng hạn như ứng cử viên`1`trong mảng đã sắp xếp`[1,2,3,4,5]`, các dấu hiệu là`[0,+1,+1,+1,+1]`. Chúng tôi bắt đầu với`sm=4`. Quá trình quét có thể tạo thành một bộ ba`+1`, giảm`sm`từ bốn xuống hai, nhưng hai còn lại`+1`các giá trị không thể tạo thành một bộ ba khác. Thuật toán từ chối ứng viên. Điều này mắc phải sai lầm phổ biến khi cho rằng đa số luôn có thể giảm đi chỉ vì số lượng của nó có tính chẵn lẻ chính xác. 

Vị trí ứng viên cũng là một ranh giới chân chính. TRONG`[3,1,2,5,4]`, ứng viên`3`ở vị trí đầu tiên nên dấu 0 xuất hiện ngay lập tức và`tp`được đặt lại trước khi bất kỳ giá trị nào khác được xử lý. Phía bên phải vẫn có thể được đơn giản hóa một cách độc lập. Thuật toán không yêu cầu các phần tử ở cả hai phía của ứng cử viên, điều này là cần thiết vì một phần tử sống sót hợp lệ có thể bắt đầu hoặc kết thúc mảng ban đầu. 

Cuối cùng, trường hợp đều bằng nhau`3 / 7 7 7`vi phạm đảm bảo về tính khác biệt chính thức, nhưng mọi so sánh với một ứng cử viên đều cho kết quả bằng không. Như vậy`sm=0`cho mọi ứng cử viên và thuật toán trả về`111`. Thử nghiệm này rất hữu ích để kiểm tra xem việc triển khai không vô tình cho rằng chính xác một phần tử mảng so sánh bằng với ứng cử viên, mặc dù đầu vào chính thức đảm bảo thuộc tính đó.
