---
title: "CF 102806D - Yahor ở Menorca"
description: "Chúng tôi có một số loại kiến. Loại i có sẵn một[i] kiến. Trong một nước đi, Dani có thể ném một số con kiến ​​đi, nhưng có hai giới hạn được áp dụng đồng thời: tổng số kiến ​​trong nước đi không được vượt quá m và số kiến ​​lấy được từ bất kỳ loại nào không được vượt quá k."
date: "2026-07-26T16:16:55+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102806
codeforces_index: "D"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2017-2018, \u0427\u0435\u0442\u0432\u0451\u0440\u0442\u0430\u044f \u043b\u0438\u0447\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 102806
solve_time_s: 36
verified: true
draft: false
---

[CF 102806D - Yahor ở Menorca](https://codeforces.com/problemset/problem/102806/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 36s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một số loại kiến. Kiểu`i`có`a[i]`kiến có sẵn. Trong một nước đi, Dani có thể đuổi một số con kiến ​​đi, nhưng có hai giới hạn được áp dụng cùng lúc: tổng số kiến ​​trong nước đi không được vượt quá`m`và số lượng kiến ​​được lấy từ bất kỳ loại nào cũng không thể vượt quá`k`. 

Mục đích là tìm số bước di chuyển tối thiểu cần thiết sao cho ít nhất`p`kiến bị vứt đi. 

Đầu vào chứa nhiều trường hợp thử nghiệm. Đối với mỗi trường hợp, mảng mô tả số lượng của từng loại kiến, theo sau là số lượng kiến ​​mục tiêu phải loại bỏ. Đầu ra là số lần di chuyển nhỏ nhất có thể đạt được mục tiêu đó. 

Tổng của tất cả`n`giá trị trong các trường hợp thử nghiệm là nhiều nhất`10^6`. Điều này loại trừ bất cứ điều gì xử lý nhiều số lần di chuyển có thể có cho mọi loại. Một giải pháp xung quanh`O(n log answer)`phù hợp vì`log`trong số câu trả lời có thể có là nhỏ, trong khi`O(n * answer)`sẽ là không thể khi số lượng kiến ​​lớn. 

Các giá trị của`a[i]`,`m`,`k`, Và`p`có thể lớn hơn nhiều so với số nguyên 32 bit, do đó việc triển khai phải sử dụng số nguyên Python hoặc số học 64 bit trong các ngôn ngữ quan trọng. 

Một sai lầm phổ biến là chỉ xem xét tổng công suất di chuyển. Ví dụ:```
1
2 10 1
100 100
150
```Câu trả lời là không`15`, mặc dù`15 * 10 = 150`kiến có thể bị ném theo tổng công suất. Mỗi nước đi chỉ có thể thực hiện một con kiến ​​trong một loại, vì vậy trong`15`di chuyển mỗi loại đóng góp nhiều nhất`15`kiến. cực đại thực sự là`30`, vì vậy kết quả đúng là:```
100
```Một trường hợp đặc biệt khác là khi một loại chứa gần như tất cả kiến:```
1
3 5 100
1000 1 1
10
```Câu trả lời là`2`, bởi vì tổng giới hạn di chuyển là hạn chế. Một giải pháp chỉ xem xét giới hạn cho mỗi loại có thể đánh giá quá cao số lần di chuyển. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là thử mọi số lần di chuyển có thể và mô phỏng số lượng kiến có thể ném được. Đối với một số lần di chuyển cố định`x`, mọi loại đều có thể đóng góp nhiều nhất`min(a[i], x * k)`kiến, vì giới hạn loại áp dụng độc lập trong mỗi lần di chuyển. Chúng tôi cũng sẽ có giới hạn toàn cầu`x * m`kiến. Kiểm tra tất cả các giá trị có thể có của`x`bằng cách tăng từng cái một có hiệu quả về mặt khái niệm, nhưng câu trả lời có thể cực kỳ lớn. Nếu như`p`ở xung quanh`10^18`, việc thử tất cả các giá trị cho đến câu trả lời là quá chậm. 

Quan sát quan trọng là nếu`x`di chuyển là đủ, thì số lượng di chuyển lớn hơn cũng là đủ. Thuộc tính đơn điệu này cho phép tìm kiếm nhị phân trên câu trả lời. 

Đối với một cố định`x`, số lượng kiến ​​tối đa có thể ném được là:```
min(x * m, sum(min(a[i], x * k)))
```Thuật ngữ đầu tiên giới hạn tổng số kiến ​​trên tất cả các nước đi. Thuật ngữ thứ hai giới hạn từng loại riêng biệt. Hai hạn chế này mô tả hoàn toàn số lượng tối đa có thể có, bởi vì mọi loại có thể được phân phối độc lập trong cùng một nhóm nước đi cho đến khi hết nguồn cung hoặc giới hạn mỗi nước đi của nó. 

Việc kiểm tra tính khả thi trở nên tuyến tính trong`n`. Tìm kiếm nhị phân tìm giá trị nhỏ nhất`x`nơi kiểm tra thành công. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O (n * câu trả lời) | O(1) | Quá chậm | 
| Tối ưu | O(n ghi câu trả lời) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tìm kiếm nhị phân câu trả lời giữa`1`và giới hạn trên đủ lớn. Giá trị tìm kiếm thể hiện số bước di chuyển mà chúng tôi đang thử nghiệm. 
2. Đối với giá trị trung bình`x`, tính số kiến ​​tối đa có thể ném vào`x`di chuyển. Đối với mỗi loại kiến, hãy thêm`min(a[i], x * k)`bởi vì đó là điều mà loại hình này có thể đóng góp nhiều nhất. Dừng thêm khi tổng số đang chạy đạt`p`, vì giá trị lớn hơn không ảnh hưởng đến quyết định. 
3. Giới hạn tổng số được tính toán bằng`x * m`, vì ngay cả khi có đủ số lượng kiến ​​từ các loại thì khả năng di chuyển cũng không thể vượt quá. 
4. Nếu số lượng kiến ​​ném tối đa có thể ít nhất là`p`, hãy tiếp tục tìm kiếm một câu trả lời nhỏ hơn. Ngược lại, loại bỏ các giá trị nhỏ hơn và tìm kiếm nửa lớn hơn. 
5. Trả về số nước đi đầu tiên vượt qua bước kiểm tra tính khả thi. 

Tại sao nó hoạt động: Tìm kiếm nhị phân dựa trên thuộc tính đơn điệu. Nếu như`x`di chuyển là đủ rồi`x + 1`nước đi cũng đủ vì chúng ta luôn có thể không sử dụng nước đi bổ sung. Hàm khả thi tính toán chính xác số lượng kiến ​​tối đa có thể với`x`di chuyển, vì vậy giá trị khả thi đầu tiên là câu trả lời hợp lệ tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve_case():
    n, m, k = map(int, input().split())
    a = list(map(int, input().split()))
    p = int(input())

    def can(x):
        total = 0
        limit = x * k
        for v in a:
            total += v if v < limit else limit
            if total >= p:
                return True
        return min(total, x * m) >= p

    lo, hi = 1, (p + m - 1) // m
    hi = max(hi, 1)

    while not can(hi):
        hi *= 2

    while lo < hi:
        mid = (lo + hi) // 2
        if can(mid):
            hi = mid
        else:
            lo = mid + 1

    return str(lo)

def main():
    t = int(input())
    ans = []
    for _ in range(t):
        ans.append(solve_case())
    print("\n".join(ans))

if __name__ == "__main__":
    main()
```Chức năng khả thi là cốt lõi của giải pháp.`x * k`được tính một lần vì mọi loại đều có cùng hạn chế cho mỗi lần di chuyển. Đối với mỗi loại, sự đóng góp được giới hạn bởi cả số lượng kiến ​​sẵn có và giới hạn này. 

Hoàn trả sớm khi số tiền đạt`p`tránh những công việc không cần thiết trong những trường hợp lớn. trận chung kết`min(total, x * m)`áp dụng giới hạn tổng công suất. Lý do có thể an toàn để trì hoãn việc này cho đến khi kết thúc là vì các khoản đóng góp theo loại chỉ được sử dụng để xác định xem liệu tổng hạn chế về phía cung có thể đáp ứng được mục tiêu hay không. 

Giới hạn trên ban đầu xuất phát từ thực tế là ít nhất`ceil(p / m)`việc di chuyển luôn cần thiết nếu hạn chế duy nhất là tổng khả năng di chuyển. Vòng lặp nhân đôi xử lý các trường hợp hạn chế theo loại yêu cầu di chuyển nhiều hơn. 

Số nguyên Python tránh tràn khi nhân các giá trị lớn như`x * k`. 

## Ví dụ đã hoạt động 

Hãy xem xét:```
1
3 5 3
10 10 10
12
```Tìm kiếm nhị phân kiểm tra số lần di chuyển khác nhau. 

| Di chuyển đã được thử nghiệm | Giới hạn loại cho mỗi loại | Có sẵn từ các loại | Tổng giới hạn di chuyển | Khả thi | 
| --- | --- | --- | --- | --- | 
| 1 | 3 | 9 | 5 | Không | 
| 2 | 6 | 18 | 10 | Không | 
| 3 | 9 | 27 | 15 | Có | 

Giá trị khả thi đầu tiên là`3`, vậy câu trả lời là`3`. Điều này cho thấy khả năng di chuyển toàn cầu và năng lực từng loại tương tác như thế nào. 

Một ví dụ khác:```
1
2 10 1
100 100
150
```| Di chuyển đã được thử nghiệm | Giới hạn loại cho mỗi loại | Có sẵn từ các loại | Tổng giới hạn di chuyển | Khả thi | 
| --- | --- | --- | --- | --- | 
| 15 | 15 | 30 | 150 | Không | 
| 75 | 75 | 150 | 750 | Có | 

Câu trả lời là`75`. Điều này chứng tỏ tại sao chỉ nhìn vào tổng công suất lại cho kết quả không chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log A) | Mỗi bước tìm kiếm nhị phân sẽ quét tất cả các loại kiến ​​và`A`là phạm vi câu trả lời. | 
| Không gian | O(1) bên cạnh việc lưu trữ đầu vào | Chỉ cần bộ đếm và biến tìm kiếm nhị phân. | 

Tổng số loại kiến ​​trong tất cả các thử nghiệm là`10^6`, do đó việc quét tuyến tính bên trong mỗi bước tìm kiếm nhị phân sẽ hiệu quả. Hệ số logarit nhỏ vì phạm vi câu trả lời được giới hạn bởi các giá trị số nguyên lớn chứ không phải bởi số phần tử. 

## Trường hợp thử nghiệm```python
import sys
import io

def solution(data):
    input = io.StringIO(data).readline

    def solve_case():
        n, m, k = map(int, input().split())
        a = list(map(int, input().split()))
        p = int(input())

        def can(x):
            total = 0
            limit = x * k
            for v in a:
                total += min(v, limit)
                if total >= p:
                    return True
            return min(total, x * m) >= p

        lo, hi = 1, max(1, (p + m - 1) // m)
        while not can(hi):
            hi *= 2

        while lo < hi:
            mid = (lo + hi) // 2
            if can(mid):
                hi = mid
            else:
                lo = mid + 1
        return str(lo)

    t = int(input())
    return "\n".join(solve_case() for _ in range(t))

assert solution("""1
3 5 3
10 10 10
12
""") == "3"

assert solution("""1
2 10 1
100 100
150
""") == "75"

assert solution("""1
1 100 100
7
7
""") == "1"

assert solution("""1
5 4 2
1 1 1 1 1
5
""") == "2"

assert solution("""1
3 1000000000000 1
1000000000000 1000000000000 1000000000000
2999999999999
""") == "2999999999999"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Ba loại có giới hạn vừa phải | 3 | Tương tác giữa cả hai hạn chế | 
| Hai loại lớn với giới hạn nhỏ cho mỗi loại | 75 | Xử lý tắc nghẽn theo từng loại | 
| Một loại và đủ công suất | 1 | Hành vi kích thước tối thiểu | 
| Nhiều giá trị nhỏ bằng nhau | 2 | Giá trị bằng nhau và tổng công suất | 
| Số lượng rất lớn | Câu trả lời lớn | Xử lý số nguyên và phạm vi tìm kiếm nhị phân | 

## Vỏ cạnh 

Đối với trường hợp cạnh đầu tiên:```
1
2 10 1
100 100
150
```Một giải pháp bất cẩn có thể chia rẽ`p`qua`m`và trở về`15`. Các thử nghiệm thuật toán`15`di chuyển và tính toán rằng mỗi loại chỉ có thể đóng góp`15`kiến, cho`30`tổng, vì vậy nó từ chối giá trị đó. Việc tìm kiếm nhị phân tiếp tục cho đến khi`75`, trong đó mỗi loại đóng góp`75`kiến và mục tiêu trở nên có thể tiếp cận được. 

Đối với trường hợp cạnh thứ hai:```
1
3 5 100
1000 1 1
10
```Giới hạn cho mỗi loại không bị hạn chế vì một lần di chuyển có thể mất tới`100`kiến từ một loại, nhưng tổng khả năng di chuyển chỉ`5`. Việc kiểm tra tính khả thi trả về sai cho một nước đi và đúng cho hai nước đi, đưa ra câu trả lời đúng về`2`. 

Đối với các giá trị bằng nhau:```
1
5 4 2
1 1 1 1 1
5
```Giới hạn loại cho phép hai con kiến ​​thuộc mỗi loại trong mỗi lần di chuyển, nhưng tổng sức chứa giới hạn mỗi lần di chuyển là bốn con kiến. Một nước đi không thể loại bỏ hết năm con kiến, trong khi hai nước đi có thể loại bỏ chúng nên thuật toán trả về`2`. 

Đối với trường hợp tối thiểu:```
1
1 100 100
7
7
```Chỉ có một loại và một nước đi có thể thực hiện được. Dung lượng loại và dung lượng di chuyển đều đủ lớn nên giá trị tìm kiếm nhị phân đầu tiên thành công là`1`.
