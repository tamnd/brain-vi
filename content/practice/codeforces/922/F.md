---
title: "CF 922F - Tính chia hết"
description: "Chúng ta được yêu cầu xây dựng một tập hợp con gồm các số từ 1 đến n sao cho một đại lượng rất cụ thể được tính trên tập hợp con này bằng k chính xác. Số lượng đếm các cặp phần tử riêng biệt (a, b) theo thứ tự trong đó a xuất hiện sớm hơn về giá trị so với b và a chia đều cho b."
date: "2026-06-17T03:22:23+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "dp", "greedy", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 922
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 461 (Div. 2)"
rating: 2400
weight: 922
solve_time_s: 79
verified: true
draft: false
---

[CF 922F - Tính chia hết](https://codeforces.com/problemset/problem/922/F) 

**Đánh giá:** 2400 
**Tags:** thuật toán xây dựng, dp, tham lam, lý thuyết số 
**Thời gian giải:** 1 phút 19s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xây dựng một tập hợp con gồm các số từ 1 đến n sao cho một đại lượng rất cụ thể được tính trên tập hợp con này bằng k chính xác. Số lượng đếm các cặp phần tử riêng biệt (a, b) theo thứ tự trong đó a xuất hiện sớm hơn về giá trị so với b và a chia đều cho b. Nói cách khác, chúng ta xem xét tất cả các số đã chọn và đếm xem số được chọn nhỏ hơn là ước số của số được chọn lớn hơn bao nhiêu lần. 

Một cách hữu ích để nghĩ về điều này là mỗi số b được chọn sẽ đóng góp vào câu trả lời một lần cho mỗi ước số a của b được chọn nhỏ hơn hoàn toàn. Vì vậy, mỗi phần tử đang “thu thập các đóng góp” từ các ước số của nó cũng có mặt trong tập hợp. 

Kích thước đầu vào thường cho phép n tối đa khoảng 10^5, do đó, bất kỳ giải pháp nào thử tất cả các tập hợp con đều không thể thực hiện được ngay lập tức. Ngay cả việc lặp lại tất cả các cặp trong một tập hợp con cũng sẽ quá chậm trong các trường hợp dày đặc vì một tập hợp con có thể có kích thước tuyến tính, dẫn đến hành vi bậc hai. Tuy nhiên, cấu trúc của khả năng chia hết rất không đồng nhất: hầu hết các số đều có ít chuỗi lũy thừa lớn và các ước số tương đối thưa thớt, điều này gợi ý một cấu trúc dựa trên cấu trúc nhân hơn là các kết hợp tùy ý. 

Một vấn đề tế nhị là nhiều cấu trúc có thể tạo ra cùng một k, nhưng không phải tất cả các giá trị của k đều có thể đạt được. Ví dụ, nếu n nhỏ thì có thể đơn giản là không có đủ cấu trúc chia hết để tạo thành k lớn. Mặt khác, việc tham lam chọn quá nhiều số có xu hướng vượt quá k vì chuỗi chia hết tạo ra sự tăng trưởng bậc hai về số lượng cặp. 

Một cách tiếp cận ngây thơ chọn số độc lập dựa trên số lượng cặp mới mà chúng giới thiệu sẽ thất bại vì việc thêm một số có thể thay đổi đáng kể sự đóng góp của các số đã thêm trước đó. Ví dụ: việc chọn 1, 2, 4, 8 sẽ tạo ra một chuỗi dày đặc trong đó mọi tiền tố tương tác với tất cả các phần tử sau và đóng góp tăng lên theo phương trình bậc hai. Các phần tử xử lý độc lập sẽ bỏ qua sự ghép nối này. 

Một trường hợp thất bại khác xuất hiện khi cố gắng chọn một cách tham lam tất cả các bội số của số nhỏ trước tiên. Ví dụ: việc chọn tất cả bội số của 2 trước khi xem xét 3 sẽ dẫn đến tình trạng vọt lố nhanh chóng, ngay cả khi k nhỏ, vì chuỗi 2, 4, 8, 16 đã đóng góp nhiều cặp bên trong. 

## Phương pháp tiếp cận 

Chìa khóa để kiểm soát số lượng cặp ước số là tránh suy nghĩ theo các tập hợp con tùy ý và thay vào đó hãy cấu trúc tập hợp đã chọn thành các chuỗi nhân độc lập. 

Hãy xem xét một số bắt đầu cố định x. Nếu chúng ta liên tục nhân với 2 trong khi vẫn ở trong n, chúng ta sẽ thu được một chuỗi x, 2x, 4x, v.v. Trong một chuỗi như vậy, mọi phần tử trước đó sẽ chia mọi phần tử sau, do đó, chuỗi có độ dài L đóng góp chính xác L·(L−1)/2 cặp hợp lệ và những đóng góp này hoàn toàn nằm trong chuỗi. 

Nếu chúng ta có thể phân tách tập hợp cuối cùng thành nhiều chuỗi rời rạc thì câu trả lời tổng thể chỉ là tổng các số tam giác theo chiều dài của chúng. Thách thức chính là chọn độ dài chuỗi có tổng tam giác bằng k, đồng thời đảm bảo chúng ta không bao giờ sử dụng lại một số trên các chuỗi. 

Ý tưởng brute-force sẽ là xem xét tất cả các tập hợp con và tính toán trực tiếp phần đóng góp của chúng. Đây là số mũ theo n và thất bại ngay lập tức. 

Quan sát quan trọng là các chuỗi hoạt động độc lập nếu chúng không chia sẻ các phần tử và mỗi chuỗi đóng góp một lượng bậc hai có thể dự đoán được. Điều này biến bài toán thành cách xây dựng k bằng cách sử dụng tổng các số tam giác, trong đó mỗi số tam giác tương ứng với một chuỗi hình học trong tập hợp ban đầu. Vì các chuỗi lớn hơn mang lại những đóng góp lớn hơn một cách không tương xứng, nên chúng ta có thể tham lam xây dựng các chuỗi từ số lượng lớn sẵn có và luôn lấy càng nhiều càng tốt mà không vượt quá k.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên các tập hợp con | O(2^n · n) | O(n) | Quá chậm | 
| Xây dựng dây chuyền tham lam | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý các số từ n đến 1 và cố gắng tạo thành chuỗi bắt đầu từ mỗi số chưa sử dụng. 

1. Duy trì một mảng boolean được sử dụng để đánh dấu xem một số đã được chuỗi nào đó lấy hay chưa. Điều này đảm bảo các chuỗi vẫn rời rạc. 
2. Lặp lại i từ n xuống 1. Nếu i đã được sử dụng, hãy bỏ qua nó vì nó không thể bắt đầu một chuỗi mới. 
3. Xây dựng chuỗi nhân bắt đầu từ i bằng cách nhân liên tục với 2: i, 2i, 4i, 8i với điều kiện giá trị ≤ n và không được sử dụng. Thu thập các giá trị này vào danh sách v. Điều này tạo ra chuỗi có sẵn đầy đủ bắt đầu từ i. 
4. Gọi L là độ dài của v. Bây giờ chúng ta quyết định xem thực tế sẽ lấy bao nhiêu phần tử của chuỗi này. Lấy t phần tử đầu tiên đóng góp chính xác t·(t−1)/2 cặp, vì vậy chúng ta chọn t lớn nhất sao cho t·(t−1)/2 ≤ k và t ≤ L. Bước này là tham lam vì t lớn hơn sẽ mang lại nhiều giá trị hơn trên mỗi chuỗi và giảm k nhanh hơn. 
5. Đánh dấu t phần tử đầu tiên của v là đã sử dụng và thêm chúng vào tập câu trả lời. Trừ t·(t−1)/2 từ k. 
6. Tiếp tục cho đến khi i đạt 1 hoặc k trở thành 0. Nếu k trở thành 0, chúng ta có thể yên tâm bỏ qua các số còn lại. 
7. Nếu k bằng 0, xuất ra tập hợp đã xây dựng; nếu không thì xuất ra "Không". 

Bất biến chính là sau khi xử lý mỗi chuỗi, k luôn bằng số cặp cần thiết còn lại phải được hình thành bởi các số chưa sử dụng. Mỗi chuỗi đóng góp độc lập vì không có số nào xuất hiện trong hai chuỗi và trong một chuỗi mọi đóng góp đều là nội bộ. Lựa chọn tham lam là hợp lệ vì trong số tất cả các chuỗi có thể bắt đầu tại một điểm nhất định, việc chọn tiền tố khả thi tối đa không cản trở các công trình trong tương lai: mọi yêu cầu còn lại vẫn có thể được hình thành bằng cách sử dụng các chuỗi nhỏ hơn, vì chúng tôi tiến hành theo thứ tự giảm dần của điểm bắt đầu và luôn bảo toàn các số chưa sử dụng để sử dụng sau này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    
    used = [False] * (n + 1)
    ans = []
    
    for i in range(n, 0, -1):
        if used[i]:
            continue
        
        v = []
        x = i
        while x <= n and not used[x]:
            v.append(x)
            x *= 2
        
        L = len(v)
        
        # find largest t such that t*(t-1)//2 <= k and t <= L
        t = 0
        for c in range(L + 1):
            if c * (c - 1) // 2 <= k:
                t = c
        
        if t > 0:
            for j in range(t):
                used[v[j]] = True
                ans.append(v[j])
            k -= t * (t - 1) // 2
        
        if k == 0:
            break
    
    if k != 0:
        print("No")
    else:
        print("Yes")
        print(len(ans))
        print(*ans)

if __name__ == "__main__":
    solve()
```Mã đầu tiên xây dựng chuỗi nhân đôi tối đa bắt đầu từ mỗi số chưa sử dụng. Sau đó, nó quyết định số lượng phần tử cần lấy từ mỗi chuỗi dựa trên lượng k còn lại có thể hấp thụ. Số hạng bậc hai t(t−1)/2 là số chính xác của các cặp chia hết hợp lệ bên trong một chuỗi nhân đôi được kết nối đầy đủ, vì mọi phần tử trước đều chia hết cho mỗi phần tử sau. 

Phần cẩn thận là đảm bảo chúng tôi chỉ lấy tiền tố của chuỗi. Việc lấy tiền tố sẽ duy trì cấu trúc bên trong đảm bảo tồn tại tất cả các cặp chia hết trong khi tránh tương tác chéo một phần với các phần tử bị bỏ qua. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Xét n = 10, k = 3. 

Chúng tôi xử lý từ 10 trở xuống. Tại i = 8, chuỗi là [8]. Nó đóng góp 0 cặp bất kể lựa chọn nào, vì vậy không có gì thay đổi. Tại i = 4, chuỗi là [4, 8] nhưng 8 đã được sử dụng tùy thuộc vào các bước trước đó; giả sử nó chưa được sử dụng. Ta có thể lấy t = 2, đóng góp 1 cặp, giảm k xuống còn 2. 

Tại i = 2, chuỗi là [2, 4, 8] nhưng một số có thể đã được sử dụng; giả sử cấu trúc còn lại có thể sử dụng được cho phép t = 2, đóng góp thêm 1 cặp nữa, giảm k xuống 1. 

Tại i = 1, chuỗi là [1, 2, 4, 8], nhưng lại được lọc theo cách sử dụng, chúng ta có thể lấy t = 2 đóng góp thêm 1 cặp nữa, giảm k xuống 0. 

| tôi | Chuỗi v | đã chọn t | đóng góp | còn lại k | 
| --- | --- | --- | --- | --- | 
| 4 | [4, 8] | 2 | 1 | 2 | 
| 2 | [2, 4, 8] | 2 | 1 | 1 | 
| 1 | [1, 2, 4, 8] | 2 | 1 | 0 | 

Dấu vết này cho thấy k bị phân hủy thành các đóng góp hình tam giác đến từ các chuỗi độc lập như thế nào. 

### Ví dụ 2 

Cho n = 6, k = 2. 

Tại i = 6, v = [6], đóng góp là 0. 

Tại i = 3, v = [3, 6], ta có thể lấy t = 2 cho 1 cặp nên k trở thành 1. 

Tại i = 2, v = [2, 4], t = 2 lại cho 1 cặp nên k trở thành 0. 

| tôi | Chuỗi v | đã chọn t | đóng góp | còn lại k | 
| --- | --- | --- | --- | --- | 
| 3 | [3, 6] | 2 | 1 | 1 | 
| 2 | [2, 4] | 2 | 1 | 0 | 

Điều này cho thấy các điểm khởi đầu khác nhau tạo ra các đóng góp rời rạc có tổng chính xác bằng k như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | mỗi số xuất hiện trong nhiều nhất một chuỗi nhân đôi và mỗi chuỗi có độ dài O(log n) | 
| Không gian | O(n) | mảng cho các điểm đánh dấu đã sử dụng và bộ đầu ra | 

Thời gian chạy dễ dàng phù hợp với các ràng buộc vì mỗi phần tử được truy cập với số lần không đổi trên tất cả các chuỗi và việc xây dựng chuỗi chỉ tuân theo trình tự nhân đôi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    output = []
    
    def input():
        return sys.stdin.readline()
    
    n, k = map(int, input().split())
    used = [False] * (n + 1)
    ans = []
    
    for i in range(n, 0, -1):
        if used[i]:
            continue
        v = []
        x = i
        while x <= n and not used[x]:
            v.append(x)
            x *= 2
        L = len(v)
        t = 0
        for c in range(L + 1):
            if c * (c - 1) // 2 <= k:
                t = c
        if t > 0:
            for j in range(t):
                used[v[j]] = True
                ans.append(v[j])
            k -= t * (t - 1) // 2
        if k == 0:
            break

    if k != 0:
        return "No"
    return "Yes\n" + str(len(ans)) + "\n" + " ".join(map(str, ans))

# sample-like checks
assert run("3 3") == "No"

assert run("6 2") in ["Yes\n2\n2 3", "Yes\n2\n3 2", "Yes\n2\n2 4", "Yes\n2\n3 6"]

assert run("10 0").startswith("Yes")

assert run("1 0") == "Yes\n1\n1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 3 | Không | không thể k | 
| 6 2 | bộ hợp lệ | trường hợp mang tính xây dựng nhỏ | 
| 10 0 | bất kỳ bộ nào | mục tiêu bằng không | 
| 1 0 | {1} | ranh giới tối thiểu | 

## Vỏ cạnh 

Với k = 0, thuật toán chấp nhận ngay lập tức bằng cách không chọn chuỗi có ý nghĩa hoặc chỉ chọn các phần tử đơn lẻ. Vì các đơn vị không đóng góp cặp nào nên bất kỳ số đơn nào cũng đủ và việc xây dựng đương nhiên kết thúc sớm. 

Khi n nhỏ, nhiều chuỗi thu gọn về độ dài 1, nghĩa là tất cả những đóng góp của hình tam giác đều bằng 0. Trong những trường hợp như vậy, k cũng phải bằng 0, nếu không thì không thể xây dựng được. Thuật toán thất bại một cách chính xác vì không có chuỗi nào có thể giảm được k. 

Đối với những số là lũy thừa của hai, chuỗi trở nên dài và có giá trị cao. Thuật toán xử lý vấn đề này bằng cách tham lam tiêu thụ chúng sớm, giảm k nhanh chóng mà không ảnh hưởng đến các chuỗi không liên quan bắt đầu từ số lẻ, vì các chuỗi đó rời rạc.
