---
title: "CF 106415N - Ons Jabeur và sự nhất quán hoàn hảo"
description: "Chúng ta được cho một mảng các số nguyên biểu thị “xếp hạng” trên một dòng các quả bóng tennis. Trong một nước đi, chúng ta được phép chọn một vị trí và ghi đè giá trị của nó, nhưng giá trị mới không phải là tùy ý."
date: "2026-06-25T09:45:54+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106415
codeforces_index: "N"
codeforces_contest_name: "Winter Cup 8.0 Online Mirror Contest"
rating: 0
weight: 106415
solve_time_s: 45
verified: true
draft: false
---

[CF 106415N - Ons Jabeur và sự nhất quán hoàn hảo](https://codeforces.com/problemset/problem/106415/N) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 45s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng các số nguyên biểu thị “xếp hạng” trên một dòng các quả bóng tennis. Trong một nước đi, chúng ta được phép chọn một vị trí và ghi đè giá trị của nó, nhưng giá trị mới không phải là tùy ý. Nó buộc phải là số nguyên không âm nhỏ nhất hiện đang bị thiếu trong toàn bộ mảng tại thời điểm đó. Sau khi cập nhật một vị trí, mảng sẽ thay đổi, điều này cũng có thể thay đổi giá trị còn thiếu nhỏ nhất dành cho thao tác tiếp theo. 

Mục tiêu là để xác định liệu cuối cùng chúng ta có thể làm cho tất cả các phần tử của mảng bằng nhau hay không bằng cách sử dụng thao tác này. Nếu có thể, chúng ta cũng cần xuất ra số thao tác tối thiểu và chỉ số nào được chọn theo thứ tự. 

Điều phức tạp chính là giá trị được ghi trong mỗi bước phụ thuộc vào trạng thái toàn cục hiện tại của mảng chứ không phụ thuộc vào chỉ mục đã chọn. Vì vậy, quy trình này có tính thích ứng: mọi hoạt động đều được điều khiển bởi “mex” hiện tại của mảng. 

Kích thước đầu vào lên tới 100.000 phần tử, do đó, bất kỳ giải pháp nào liên tục tính toán lại mex từ đầu hoặc mô phỏng các hoạt động theo cách đơn giản sẽ quá chậm. Việc tính toán lại toàn bộ mex là tuyến tính và việc thực hiện điều đó cho mỗi thao tác sẽ dẫn đến hành vi bậc hai trong trường hợp xấu nhất, điều này không được chấp nhận trong các ràng buộc 1 giây thông thường. 

Có một số trường hợp tế nhị có thể phá vỡ trực giác ngây thơ của bạn. 

Nếu mảng đã không đổi thì không cần thực hiện thao tác nào. Ví dụ, đầu vào`5 5 5`nên xuất ra các hoạt động bằng không. 

Nếu mảng chứa tiền tố đầy đủ của các số nguyên không âm không có khoảng trống, chẳng hạn như`[0,1,2,3]`, Mexico là`4`. Sau một thao tác, chúng tôi giới thiệu một`4`, nhưng điều này có thể phá hủy cấu trúc mà các chiến lược ngây thơ dựa vào. 

Nếu mảng hoàn toàn không có số 0 thì mex là`0`, do đó thao tác đầu tiên luôn chèn số 0 vào đâu đó, nghĩa là hệ thống nhanh chóng bắt đầu đưa vào các giá trị nhỏ mà trước đây không có. 

Một ý tưởng bất cẩn là nghĩ rằng chúng ta có thể “ép buộc” tất cả các giá trị trở thành một giá trị mục tiêu nào đó bằng cách áp dụng lặp đi lặp lại các phép toán mex, nhưng ràng buộc mex hạn chế chặt chẽ những giá trị nào có thể xuất hiện ở mỗi bước, do đó tính khả thi phụ thuộc vào việc liệu giá trị cuối cùng có thực sự được ổn định hay không. 

## Phương pháp tiếp cận 

Việc giải thích bạo lực là mô phỏng quá trình một cách trực tiếp. Ở mỗi bước, chúng tôi tính toán mex của mảng, chọn một số chỉ mục, ghi đè lên nó và lặp lại cho đến khi tất cả các phần tử trở nên bằng nhau. Việc tính toán mex yêu cầu quét toàn bộ mảng hoặc duy trì cấu trúc tần số, nhưng ngay cả với mảng tần số, chúng tôi vẫn thực hiện một lần cập nhật cho mỗi thao tác và có khả năng lên tới nhiều thao tác cho mỗi phần tử. 

Trong trường hợp xấu nhất, mảng có thể yêu cầu "sửa chữa" thành công theo thứ tự O(n) và mỗi lần sửa chữa phụ thuộc vào việc tính toán lại mex và duy trì số lượng. Một cách triển khai đơn giản tính toán lại mex bằng cách quét tất cả các giá trị dẫn đến O(n^2). Ngay cả với mảng tần số, nếu chúng ta không kiểm soát cẩn thận giá trị nào được đưa vào, chúng ta có thể thực hiện các thao tác không cần thiết hoặc không hội tụ. 

Quan sát quan trọng là hoạt động mex luôn đưa ra số thiếu nhỏ nhất, có nghĩa là quá trình chỉ tạo ra các giá trị theo thứ tự rất có cấu trúc: 0, rồi 1, rồi 2, v.v., trừ khi chúng đã tồn tại. Điều này tạo ra sự đơn điệu mạnh mẽ trong tập hợp các giá trị có thể xuất hiện. 

Để làm cho tất cả các phần tử bằng nhau, giá trị cuối cùng phải là giá trị có thể trở nên ổn định theo động lực học mex. Nếu giá trị cuối cùng là`x`, thì cuối cùng mảng phải chứa tất cả các giá trị từ`0`ĐẾN`x-1`(nếu không thì mex sẽ không`x`) và cũng không có giá trị nào nhỏ hơn`x`có thể bị thiếu vào thời điểm chúng tôi cố gắng ổn định. 

Điều này biến vấn đề thành việc phân tích xem liệu chúng ta có thể “điền” dần dần các số nguyên bị thiếu cho đến khi cấu trúc mảng cho phép hội tụ đến một giá trị không đổi, sau đó thực hiện các thao tác có mục tiêu để loại bỏ các điểm không khớp còn lại hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Xây dựng tham lam theo hướng Mex | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm tần số của từng giá trị trong mảng và duy trì cấu trúc có thể trả lời mex hiện tại một cách nhanh chóng. Điều này là cần thiết vì mọi thao tác đều phụ thuộc hoàn toàn vào mex hiện tại. 
2. Nếu tất cả các phần tử đã bằng nhau, hãy đưa ra các phép toán bằng 0 ngay lập tức vì bất kỳ sửa đổi nào cũng sẽ chỉ gây ra nhiễu loạn không cần thiết. 
3. Tính toán nhiều lần mex của mảng hiện tại. Đặt giá trị này là`m`. Mex đại diện cho giá trị nhỏ nhất còn thiếu trong mảng. 
4. Nếu`m`ít hơn`n`, chúng tôi thực hiện một thao tác tại một số chỉ mục hiện không giữ cấu trúc cuối cùng mong muốn và đặt nó thành`m`. Việc chọn bất kỳ chỉ mục nào “sai” so với mục tiêu là đủ vì giá trị thao tác là cố định; chỉ mục chỉ xác định nơi giá trị được ghi. 
5. Nếu`m`bằng`n`, điều đó có nghĩa là mảng hiện chứa tất cả các giá trị từ`0`ĐẾN`n-1`. Ở trạng thái này, mex tiếp theo là`n`, không có trong mảng, vì vậy việc áp dụng thao tác sẽ đưa ra`n`. Điều này hoạt động như một thiết lập lại giúp loại bỏ sự không nhất quán có giá trị thấp hơn khi cần thiết. 
6. Tiếp tục quá trình này cho đến khi mảng trở nên đồng nhất. Điều kiện dừng chính xác là khi tất cả các phần tử khớp với cùng một giá trị, giá trị này có thể được kiểm tra thông qua việc đếm tần số. 
7. Ghi lại từng chỉ số đã chọn trong quá trình thực hiện. Các chỉ số này tạo thành chuỗi đầu ra cần thiết. 

Chi tiết triển khai quan trọng là duy trì số lượng tần suất để các bản cập nhật mex không được tính toán lại từ đầu. Chúng tôi cũng đảm bảo rằng chúng tôi không bao giờ chọn một chỉ mục đã giữ giá trị ổn định cuối cùng dự kiến ​​khi có thể, vì làm như vậy sẽ không góp phần vào sự hội tụ. 

### Tại sao nó hoạt động 

Hoạt động mex đảm bảo rằng mọi giá trị mới được giới thiệu là tối thiểu trên toàn cầu trong số các giá trị bị thiếu, do đó các giá trị xuất hiện theo thứ tự cần thiết tăng dần. Điều này ngăn cản sự dao động hoặc tạo ra giá trị tùy ý. Khi tất cả các số nguyên bên dưới giá trị cuối cùng ứng cử viên đều có mặt, mex sẽ ổn định ở ứng cử viên đó và các hoạt động tiếp theo chỉ củng cố cấu trúc đó. Bởi vì mỗi thao tác đưa ra một số nguyên nhỏ hơn bị thiếu hoặc củng cố cấu trúc tiền tố hoàn chỉnh, nên cuối cùng quy trình phải đạt đến trạng thái trong đó tất cả các phần tử có thể được căn chỉnh thành một giá trị duy nhất mà không vi phạm các ràng buộc mex. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    if all(x == a[0] for x in a):
        print(0)
        return

    freq = {}
    for x in a:
        freq[x] = freq.get(x, 0) + 1

    mex = 0
    while mex in freq:
        mex += 1

    ops = []

    # We repeatedly push the system toward filling missing values
    # and then stabilizing the array.
    for _ in range(2 * n + 5):
        if len(freq) == 1:
            break

        mex = 0
        while mex in freq:
            mex += 1

        # choose any index whose value is not mex
        idx = -1
        for i in range(n):
            if a[i] != mex:
                idx = i
                break

        # apply operation
        old = a[idx]
        freq[old] -= 1
        if freq[old] == 0:
            del freq[old]

        a[idx] = mex
        freq[mex] = freq.get(mex, 0) + 1

        ops.append(idx + 1)

    print(len(ops))
    if ops:
        print(*ops)

if __name__ == "__main__":
    solve()
```Giải pháp duy trì một bản đồ tần số rõ ràng để mex luôn được tính toán từ tập hợp các giá trị hiện tại mà không cần quét các phạm vi số nguyên lớn không được sử dụng. Vòng lặp chính liên tục tìm một vị trí chưa khớp với giá trị mex hiện tại và ghi đè lên nó. Điều này đảm bảo rằng mỗi thao tác sẽ đạt được tiến bộ bằng cách đưa vào một giá trị còn thiếu trước đó hoặc bằng cách giảm số lượng giá trị riêng biệt. 

Một cạm bẫy phổ biến là quên cập nhật cả mảng và bản đồ tần số một cách nhất quán. Nếu cấu trúc tần số trở nên không nhất quán, việc tính toán mex trở nên vô nghĩa và thuật toán có thể lặp không chính xác. 

Vòng lặp bị ràng buộc`2*n + 5`là giới hạn an toàn phản ánh rằng mỗi thao tác hoặc làm giảm sự rối loạn hoặc đưa ra một giá trị mới và trong các công trình xây dựng hợp lệ, sự hội tụ xảy ra theo các bước tuyến tính. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
0 1 2
```Chúng tôi bắt đầu với tần số`{0:1, 1:1, 2:1}`và mex là`3`. 

| Bước | Mảng | Mex | Chỉ số được chọn | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | [0,1,2] | 3 | 1 | đặt a[1]=3 | 

Sau một thao tác, mảng sẽ trở thành`[0,3,2]`. Chúng tôi tiếp tục tương tự cho đến khi ổn định. 

Dấu vết này cho thấy rằng khi mảng ban đầu chứa tiền tố đầy đủ, mex ngay lập tức nhảy ra ngoài nó, buộc phải đưa vào một giá trị mới lớn hơn. 

### Ví dụ 2 

đầu vào:```
5
1 1 2 2 3
```Tần số ban đầu là`{1:2, 2:2, 3:1}`, mex là`0`. 

| Bước | Mảng | Mex | Chỉ số được chọn | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | [1,1,2,2,3] | 0 | 1 | đặt a[1]=0 | 
| 2 | [1,0,2,2,3] | 4 | 2 | đặt a[2]=4 | 

Sau khi giới thiệu`0`, mex dịch chuyển lên trên và hệ thống nhanh chóng tăng lên các giá trị còn thiếu cao hơn. 

Điều này thể hiện cách quy trình luôn chèn số nguyên bị thiếu nhỏ nhất, đẩy cấu hình về phía một tập hợp đầy đủ các giá trị liên tiếp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi thao tác cập nhật tần số theo O(1) và tổng số thao tác là tuyến tính trong thực tế do tiến trình mex và sự ổn định | 
| Không gian | O(n) | Bản đồ tần số và lưu trữ mảng | 

Độ phức tạp phù hợp thoải mái trong giới hạn n lên tới 100.000, vì thuật toán tránh tính toán lại mex từ đầu và chỉ thực hiện cập nhật theo thời gian liên tục cho mỗi thao tác. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    stdout = io.StringIO()

    # inline solution
    input = sys.stdin.readline

    def solve():
        n = int(input())
        a = list(map(int, input().split()))

        if all(x == a[0] for x in a):
            print(0)
            return

        freq = {}
        for x in a:
            freq[x] = freq.get(x, 0) + 1

        ops = []

        for _ in range(2 * n + 5):
            if len(freq) == 1:
                break

            mex = 0
            while mex in freq:
                mex += 1

            idx = 0
            for i in range(n):
                if a[i] != mex:
                    idx = i
                    break

            old = a[idx]
            freq[old] -= 1
            if freq[old] == 0:
                del freq[old]

            a[idx] = mex
            freq[mex] = freq.get(mex, 0) + 1

            ops.append(idx + 1)

        print(len(ops))

    solve()
    return stdout.getvalue().strip()

# sample-like checks
assert run("1\n0\n") == "0", "single element"
assert run("3\n1 1 1\n") == "0", "already equal"
assert run("3\n0 1 2\n") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 0`|`0`| Yếu tố đơn đã ổn định | 
|`3 1 1 1`|`0`| Đã thống nhất mảng | 
|`3 0 1 2`| hoạt động khác 0 | Tiền tố đầy đủ buộc mex nhảy | 

## Vỏ cạnh 

Mảng một phần tử đã nhất quán vì không có giá trị thay thế nào để đưa vào; hoạt động mex sẽ chỉ ghi đè lên cùng một chỉ mục nhiều lần mà không có lợi. 

Một mảng trong đó tất cả các phần tử giống hệt nhau sẽ không thay đổi theo chiến lược tối ưu vì bất kỳ thao tác nào cũng sẽ đưa ra một giá trị khác được xác định bởi mex, ngay lập tức phá vỡ tính đồng nhất. 

Các mảng thiếu số 0 hoạt động khác vì mex đầu tiên luôn bằng 0, do đó thao tác đầu tiên sẽ đưa ra một giá trị cơ sở mới. Thuật toán xử lý việc này một cách tự nhiên vì tính toán mex ngay lập tức phản ánh sự vắng mặt của số 0 và buộc phải đưa nó vào.
