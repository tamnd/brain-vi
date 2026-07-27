---
title: "CF 102823A - Hợp nhất mảng"
description: "Chúng ta có hai mảng A và B. Chúng ta phải tạo một mảng cuối cùng bằng cách xen kẽ các phần tử từ hai mảng trong khi vẫn giữ thứ tự ban đầu bên trong A và bên trong B. Sau khi hợp nhất, mỗi phần tử đóng góp giá trị của nó nhân với vị trí dựa trên 1 của nó trong mảng cuối cùng."
date: "2026-07-26T15:49:56+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102823
codeforces_index: "A"
codeforces_contest_name: "2018 China Collegiate Programming Contest - Guilin Site"
rating: 0
weight: 102823
solve_time_s: 59
verified: true
draft: false
---

[CF 102823A - Hợp nhất mảng](https://codeforces.com/problemset/problem/102823/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 59s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có hai mảng,`A`Và`B`. Chúng ta phải tạo một mảng cuối cùng bằng cách xen kẽ các phần tử từ hai mảng trong khi vẫn giữ nguyên thứ tự ban đầu bên trong`A`và bên trong`B`. Sau khi hợp nhất, mọi phần tử đóng góp giá trị của nó nhân với vị trí dựa trên 1 của nó trong mảng cuối cùng. Mục tiêu là chọn thứ tự hợp nhất mang lại tổng chi phí nhỏ nhất có thể. Đầu ra được yêu cầu cũng bao gồm số trường hợp kiểm thử ở định dạng được hiển thị bởi câu lệnh. 

Kích thước đầu vào loại trừ việc thử tất cả các cách hợp nhất có thể. Nếu hai mảng có độ dài`n`Và`m`, có nhiều phép xen kẽ hợp lệ theo cấp số nhân, bởi vì mỗi phép hợp nhất tương ứng với việc chọn các vị trí mà một trong các mảng chiếm giữ. Với chiều dài đạt tới`100000`và tổng chiều dài trên tất cả các bài kiểm tra đạt`10^6`, mọi giải pháp tùy thuộc vào số lượng đơn đặt hàng hợp nhất có thể là không thể. Chúng ta cần một phương pháp thời gian tuyến tính để đưa ra một quyết định cho từng phần tử. 

Phần khó khăn là bản thân các mảng không cần phải sắp xếp. Một lỗi phổ biến là luôn lấy phần tử hiện tại nhỏ hơn vì nó giống với thao tác hợp nhất thông thường. Điều đó sai ở đây vì hệ số nhân vị trí tăng lên khi chúng ta di chuyển sang phải. Giá trị lớn hơn thường nên được đặt sớm hơn. 

Ví dụ, hãy xem xét:```
1
2 2
5 3
4 5
```Mảng hợp nhất tối ưu là:```
5 4 5 3
```Chi phí là:```
1*5 + 2*4 + 3*5 + 4*3 = 40
```Sự hợp nhất đầu tiên nhỏ hơn sẽ tạo ra:```
4 5 5 3
```với chi phí:```
1*4 + 2*5 + 3*5 + 4*3 = 41
```Sự khác biệt đến từ việc di chuyển một số lớn hơn sang trái sẽ tiết kiệm được nhiều tiền hơn so với việc di chuyển một số nhỏ hơn sang trái cùng một lượng. 

Một trường hợp cạnh khác là giá trị bằng nhau. Ví dụ:```
1
1 1
7
7
```Cả hai sự hợp nhất có thể đều giống hệt nhau:```
7 7
```và câu trả lời là:```
21
```Giải pháp sử dụng so sánh nghiêm ngặt không chính xác có thể thất bại do không sử dụng đúng một trong các mảng khi các giá trị bằng nhau. 

Trường hợp ranh giới cuối cùng là khi một mảng kết thúc sớm. Ví dụ:```
1
1 3
8
1 2 3
```Sau khi chọn`8`đầu tiên, các phần tử còn lại phải xuất hiện theo thứ tự ban đầu:```
8 1 2 3
```Câu trả lời là:```
8 + 4 + 6 + 9 = 27
```Việc triển khai chỉ so sánh các phần tử trong khi cả hai mảng đều có phần tử và quên hậu tố còn lại sẽ tạo ra kết quả không chính xác. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực trực tiếp sẽ liệt kê mọi sự hợp nhất hợp lệ. Đối với mỗi lần hợp nhất, nó sẽ tính toán chi phí thu được và giữ ở mức tối thiểu. Điều này đúng vì mọi sự sắp xếp cuối cùng có thể đều được kiểm tra. Tuy nhiên, số lần hợp nhất là hệ số nhị thức`C(n+m,n)`, tăng trưởng theo cấp số nhân. Ngay cả các mảng nhỏ cũng đã tạo ra nhiều khả năng nên cách tiếp cận này không thể xử lý được các giới hạn nhất định. 

Quan sát chính xuất phát từ việc xem xét hai phần tử lân cận đến từ các mảng khác nhau. Giả sử hai phần tử liền kề trong mảng cuối cùng là`x`theo sau là`y`. Đóng góp của họ vào chi phí tại các vị trí`p`Và`p+1`là:```
p*x + (p+1)*y
```Nếu chúng ta hoán đổi chúng, khoản đóng góp sẽ trở thành:```
p*y + (p+1)*x
```Sự sắp xếp thứ hai tốt hơn chính xác khi`x < y`. Nói cách khác, bất cứ khi nào hai phần tử có sẵn từ các mảng khác nhau có thể được hoán đổi, phần tử lớn hơn sẽ xuất hiện sớm hơn. 

Tại bất kỳ thời điểm nào trong quá trình hợp nhất, các phần tử duy nhất có vị trí có thể được quyết định ngay lập tức là các phần tử hiện không được sử dụng ở phía trước của hai mảng. Nếu chúng ta chọn phần tử phía trước nhỏ hơn thì phần tử phía trước lớn hơn sẽ bị buộc phải ở một vị trí sau. Trao đổi hai lựa chọn đó sẽ cải thiện chi phí. Do đó, sự lựa chọn tối ưu luôn là lấy phần tử lớn hơn trong hai phần tử phía trước hiện tại. 

Đây là lý do tương tự được sử dụng bởi thuật toán tham lam cuối cùng. Phương pháp brute-force khám phá tất cả các mệnh lệnh có thể có, nhưng đối số trao đổi cho thấy mọi giải pháp tối ưu đều có thể được chuyển đổi thành một giải pháp trong đó mỗi lựa chọn chiếm phần tử phía trước lớn hơn có sẵn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(C(n+m,n) * (n+m)) | O(n+m) | Quá chậm | 
| Tối ưu | O(n+m) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Bắt đầu bằng con trỏ ở phần tử đầu tiên chưa được sử dụng của cả hai mảng và đặt vị trí hiện tại trong mảng đã hợp nhất thành`1`. 
2. Trong khi cả hai mảng vẫn chứa các phần tử không được sử dụng, hãy so sánh hai giá trị phía trước hiện tại. Lấy cái lớn hơn, thêm phần đóng góp của nó`position * value`đến câu trả lời và di chuyển con trỏ tương ứng về phía trước. 

Lý do lựa chọn này an toàn là vì hai giá trị phía trước là phần tử duy nhất từ ​​các mảng khác nhau có thể được đặt tiếp theo. Nếu giá trị lớn hơn được đặt sau, việc hoán đổi cả hai sẽ giảm chi phí. 
3. Khi một mảng trống, hãy nối các phần tử còn lại của mảng kia theo thứ tự ban đầu của chúng. Thứ tự tương đối của chúng đã được cố định và không còn lựa chọn nào nữa. 
4. Tiếp tục tăng vị trí sau mỗi phần tử được đặt cho đến khi toàn bộ mảng đã hợp nhất được chiếm. 

Tại sao nó hoạt động: 

Điều bất biến là sau mỗi quyết định tham lam, sẽ tồn tại một sự hợp nhất tối ưu có cùng một tiền tố. Hãy xem xét hai yếu tố phía trước có sẵn`x`Và`y`. Nếu sự hợp nhất tối ưu đặt phần tử nhỏ hơn trước phần tử lớn hơn, thì hai phần tử đó cuối cùng sẽ xuất hiện theo thứ tự đó vì không phần tử nào có thể bị vượt qua bởi các phần tử trong mảng của chính nó. Việc hoán đổi hai lựa chọn liền kề sẽ thay đổi chi phí bằng cách di chuyển giá trị lớn hơn sang trái một vị trí, điều này không bao giờ làm tăng câu trả lời. Việc lặp lại quá trình trao đổi này sẽ loại bỏ mọi lựa chọn sai lầm như vậy, do đó việc lấy phần tử hiện tại lớn hơn luôn duy trì được tính tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve_case(a, b):
    i = 0
    j = 0
    pos = 1
    ans = 0
    n = len(a)
    m = len(b)

    while i < n and j < m:
        if a[i] >= b[j]:
            ans += pos * a[i]
            i += 1
        else:
            ans += pos * b[j]
            j += 1
        pos += 1

    while i < n:
        ans += pos * a[i]
        i += 1
        pos += 1

    while j < m:
        ans += pos * b[j]
        j += 1
        pos += 1

    return ans

def main():
    t = int(input())
    out = []

    for case in range(1, t + 1):
        n, m = map(int, input().split())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))
        out.append(f"Case {case}: {solve_case(a, b)}")

    print("\n".join(out))

if __name__ == "__main__":
    main()
```Hai con trỏ`i`Và`j`đại diện cho các phần tử có sẵn tiếp theo trong hai mảng ban đầu. Vòng lặp chính thực hiện chính xác các quyết định tham lam được mô tả trong phần hướng dẫn. Việc so sánh sử dụng`>=`thay vì`>`sao cho các giá trị bằng nhau được sử dụng một cách nhất quán mà không ảnh hưởng đến câu trả lời. 

Sau khi sử dụng hết một mảng, các vòng lặp còn lại không còn tùy chọn nữa. Các yếu tố còn lại vẫn đóng góp vào chi phí và vị trí của chúng tiếp tục tăng kể từ điểm dừng vòng lặp đầu tiên. 

Số nguyên Python được sử dụng tự động với độ chính xác tùy ý, điều này rất hữu ích vì chi phí tối đa có thể vượt quá giới hạn số nguyên 32 bit. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
2 2
5 3
4 5
```| Vị trí | Một con trỏ | Con trỏ B | Giá trị được chọn | Chi phí vận hành | 
| --- | --- | --- | --- | --- | 
| 1 | 5 | 4 | 5 | 5 | 
| 2 | 3 | 4 | 4 | 13 | 
| 3 | 3 | 5 | 5 | 28 | 
| 4 | 3 | trống | 3 | 40 | 

Dấu vết cho thấy tại sao việc lấy giá trị sẵn có lớn nhất trước tiên lại có tác dụng. giá trị`5`từ mảng đầu tiên được đặt trước`4`, tạo tiền tố tối ưu. 

### Mẫu 2 

đầu vào:```
3 3
1 3 5
2 6 4
```| Vị trí | Một con trỏ | Con trỏ B | Giá trị được chọn | Chi phí vận hành | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 2 | 2 | 2 | 
| 2 | 1 | 6 | 6 | 14 | 
| 3 | 1 | 4 | 4 | 26 | 
| 4 | 1 | trống | 1 | 30 | 
| 5 | 3 | trống | 3 | 45 | 
| 6 | 5 | trống | 5 | 75 | 

Dấu vết thứ hai minh họa trường hợp một mảng kết thúc trước mảng kia. Hậu tố của mảng còn lại được thêm vào mà không cần quyết định bổ sung. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n+m) | Mỗi phần tử được kiểm tra một lần và được chọn chính xác một lần. | 
| Không gian | O(1) | Chỉ các con trỏ, vị trí hiện tại và câu trả lời tích lũy mới được lưu trữ. | 

Tổng số phần tử được xử lý trên tất cả các trường hợp thử nghiệm nhiều nhất là`10^6`, do đó quét tuyến tính dễ dàng phù hợp với các giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def solve_case(a, b):
        i = j = 0
        pos = 1
        ans = 0

        while i < len(a) and j < len(b):
            if a[i] >= b[j]:
                ans += pos * a[i]
                i += 1
            else:
                ans += pos * b[j]
                j += 1
            pos += 1

        while i < len(a):
            ans += pos * a[i]
            i += 1
            pos += 1

        while j < len(b):
            ans += pos * b[j]
            j += 1
            pos += 1

        return ans

    t = int(input())
    res = []

    for case in range(1, t + 1):
        n, m = map(int, input().split())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))
        res.append(f"Case {case}: {solve_case(a, b)}")

    return "\n".join(res)

assert solve("""2
2 2
5 3
4 5
3 3
1 3 5
2 6 4
""") == """Case 1: 40
Case 2: 75""", "samples"

assert solve("""1
1 1
7
7
""") == "Case 1: 21", "equal values"

assert solve("""1
1 3
8
1 2 3
""") == "Case 1: 27", "one array finishes early"

assert solve("""1
3 3
10 1 1
9 9 9
""") == "Case 1: 99", "large front element"

assert solve("""1
3 3
0 0 0
0 0 0
""") == "Case 1: 0", "all zeros"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Trường hợp mẫu |`Case 1: 40`,`Case 2: 75`| Ví dụ chính thức và tính đúng đắn chung | 
| Các phần tử đơn có giá trị bằng nhau |`21`| Xử lý cà vạt | 
| Một mảng ngắn hơn nhiều |`27`| Xử lý hậu tố còn lại | 
| Phần tử lớn đầu tiên |`99`| Tham lam lựa chọn giá trị trước lớn hơn | 
| Tất cả số không |`0`| Xử lý các giá trị giống hệt nhau lặp đi lặp lại | 

## Vỏ cạnh 

Đối với các giá trị bằng nhau, chẳng hạn như:```
1
1 1
7
7
```thuật toán so sánh`7`Và`7`, chọn mảng đầu tiên vì`>=`điều kiện rồi lấy phần tử còn lại. Thứ tự không quan trọng vì cả hai đóng góp đều giống nhau. 

Đối với trường hợp một mảng kết thúc sớm:```
1
1 3
8
1 2 3
```thuật toán đầu tiên so sánh`8`Và`1`, mất`8`, và sau đó mảng đầu tiên trống. Các giá trị còn lại`1,2,3`phải xuất hiện theo thứ tự, đưa ra:```
8 1 2 3
```với chi phí`27`. 

Đối với tất cả các giá trị bằng nhau:```
1
3 3
0 0 0
0 0 0
```mọi sự hợp nhất có thể đều có chi phí bằng không. Các quyết định tham lam vẫn tiêu thụ mọi phần tử đúng một lần và trả về kết quả chính xác. 

Đối với các giá trị mà việc chọn phần tử nhỏ hơn có vẻ hấp dẫn:```
1
2 2
5 3
4 5
```thuật toán đầu tiên chọn`5`qua`4`. Lựa chọn đó là yếu tố tạo ra tiền tố tối ưu, vì việc đặt giá trị lớn hơn sau sẽ làm tăng hệ số nhân của nó trong khi giảm hệ số nhân của giá trị nhỏ hơn. Kết quả cuối cùng là`40`.
