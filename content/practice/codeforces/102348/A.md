---
title: "CF 102348A - Thẻ vàng"
description: "Có hai đội bóng đá có cầu thủ (a1) và (a2). Một cầu thủ của đội một bị loại sau khi nhận (k1) thẻ vàng, trong khi một cầu thủ của đội thứ hai bị loại sau khi nhận (k2) thẻ vàng."
date: "2026-08-17T10:34:57+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102348
codeforces_index: "A"
codeforces_contest_name: "ICPC 2019-2020 NERC (NEERC), Southern and Volga Russia Qualifier"
rating: 0
weight: 102348
solve_time_s: 418
verified: false
draft: false
---

[CF 102348A - Thẻ vàng](https://codeforces.com/problemset/problem/102348/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 6 phút 58 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Có hai đội bóng đá có (a_1) và (a_2) cầu thủ. Một cầu thủ của đội một bị loại sau khi nhận (k_1) thẻ vàng, trong khi một cầu thủ của đội thứ hai bị loại sau khi nhận (k_2) thẻ. Chúng tôi chỉ biết tổng số (n) thẻ vàng được đưa ra chứ không biết cầu thủ nào đã nhận chúng. 

Nhiệm vụ là tìm ra hai thái cực. Số đầu tiên là số lượng người chơi bị loại bỏ nhỏ nhất có thể trên tất cả các cách hợp lệ để phân phối (n) thẻ. Số thứ hai là số lượng người chơi bị loại bỏ lớn nhất có thể. Vấn đề còn được gọi là Codeforces 1215A, với cùng một tuyên bố và các ràng buộc. 

Số lượng người chơi và ngưỡng bài nhiều nhất là (1000) nên tổng số bài nhiều nhất 

[ 
1000\cdot1000+1000\cdot1000=2\cdot10^6. 
] 

Điều đó làm cho việc quét tuyến tính trên (n) số lượng thẻ có thể khả thi ở ngôn ngữ cấp thấp, nhưng điều đó là không cần thiết. Cấu trúc của hai đội cho phép chúng tôi rút gọn cả hai câu trả lời về một số lượng phép tính số học không đổi. Quan trọng hơn, việc liệt kê các bài tập thẻ thực tế là hoàn toàn không thể vì có thể có tới (2000) người nhận cho mỗi thẻ trong số tối đa (2\cdot10^6) thẻ. 

Ranh giới phức tạp đầu tiên là khi mọi người chơi đều có thể nhận được (k_i-1) thẻ mà không cần rời đi. Ví dụ,```
1
1
2
3
3
```Người chơi đầu tiên có thể nhận được một thẻ một cách an toàn và người thứ hai có thể nhận được hai thẻ một cách an toàn, vì vậy cả ba thẻ có thể được chỉ định mà không cần ai rời đi. Câu trả lời là`0 1`. Một phép tính tối thiểu bất cẩn sử dụng (a_1k_1+a_2k_2) làm dung lượng an toàn cũng sẽ trả về 0 ở đây, nhưng nó sẽ thất bại ngay khi (n) vượt quá dung lượng an toàn được xác định không chính xác đó. Giới hạn an toàn liên quan luôn nhỏ hơn ngưỡng loại bỏ một đơn vị. 

Một trường hợp cạnh khác xảy ra khi ngưỡng là (1). Ví dụ,```
1
1
1
5
1
```Cầu thủ đội 1 bị phạt thẻ vàng duy nhất nên đáp án là`1 1`. Sử dụng (k_1-1=0) đúng có nghĩa là người chơi đầu tiên không có quân bài an toàn nào cả. 

Trường hợp quan trọng thứ ba là khi hai ngưỡng này khác nhau. Coi như```
2
3
5
1
8
```Mỗi người chơi ở đội thứ hai bị loại bỏ một thẻ duy nhất, do đó, mức tối đa đạt được bằng cách đưa một thẻ cho mỗi người trong số ba người chơi ở đội thứ hai và năm thẻ cho một người chơi ở đội một. Điều đó mang lại bốn lần loại bỏ, phù hợp với đầu ra mẫu`0 4`. Nếu chúng ta tham lam sử dụng đội có ngưỡng lớn hơn trước, chúng ta sẽ lãng phí thẻ và bỏ lỡ mức tối đa. 

Cuối cùng, tổng số thẻ có thể bằng tổng sức chứa của tất cả người chơi. Vì```
3
1
6
7
25
```có chính xác (3\cdot6+1\cdot7=25) thẻ có thể. Mỗi người chơi phải nhận được số lượng thẻ ngưỡng của mình nên cả bốn người chơi đều rời đi và câu trả lời là`4 4`. 

## Phương pháp tiếp cận 

Một giải pháp hoàn toàn bạo lực có thể xem xét mọi người có thể nhận thẻ vàng. Với (a_1+a_2) người chơi và (n) thẻ, điều này tạo ra ((a_1+a_2)^n) các bài tập có thể thực hiện được. Ở giới hạn lớn nhất là (2000^{2.000.000}), điều này không khả thi từ xa. Cách tiếp cận này đúng vì mọi phân phối có thể đều được xem xét, nhưng không gian tìm kiếm của nó tăng theo cấp số nhân theo số lượng thẻ. 

Một cách tiếp cận bạo lực hơn một chút là liệt kê xem có bao nhiêu (n) thẻ thuộc về đội đầu tiên. Đối với giá trị được chọn (x), số lượng người chơi bị loại bỏ tối đa là 

[ 
\min(a_1,\lfloor x/k_1\rfloor) 
+ 
\min(a_2,\lfloor(n-x)/k_2\rfloor). 
] 

Việc thử mọi (x) sẽ mất (O(n)) thời gian, tối đa vẫn là khoảng hai triệu lần lặp theo các ràng buộc này và có thể hoạt động. Tuy nhiên, bài toán có cấu trúc tham lam mạnh hơn nên không có lý do gì để thực hiện phép liệt kê này. 

Ở mức tối thiểu, hãy tưởng tượng việc phân phát thẻ lần đầu tiên một cách an toàn nhất có thể. Mọi người chơi ở đội một có thể nhận được (k_1-1) thẻ mà không bị xóa và mọi người chơi ở đội thứ hai có thể nhận được (k_2-1). Do đó, số lượng thẻ có thể được hấp thụ mà không cần loại bỏ là 

[ 
S=a_1(k_1-1)+a_2(k_2-1). 
] 

Nếu (n\le S), không ai phải rời đi. Nếu (n>S), mỗi lá bài bổ sung sẽ buộc thêm một người chơi đạt đến ngưỡng loại bỏ của họ. Do đó tối thiểu là 

[ 
\max(0,n-S). 
] 

Quan sát chính cho mức tối đa là khác nhau. Người chơi bị loại sẽ tiêu thụ chính xác (k_i) thẻ. Để loại bỏ càng nhiều người chơi càng tốt, trước tiên chúng ta nên chia bài cho những người chơi cần ít thẻ hơn. Nếu (k_1<k_2), việc loại bỏ một cầu thủ ở đội một sẽ tốn ít thẻ hơn so với việc loại bỏ một cầu thủ ở đội thứ hai, do đó, tất cả các lần loại bỏ đội một với giá phải chăng phải được thực hiện trước khi sử dụng thẻ để loại bỏ đội thứ hai. Nếu (k_2<k_1), vai trò sẽ bị đảo ngược. Khi các ngưỡng bằng nhau thì thứ tự nào cũng tương đương. 

Giả sử ngưỡng nhỏ hơn là (k_s), với (a_s) người chơi tương ứng và ngưỡng còn lại là (k_l), với (a_l) người chơi. Chúng ta có thể loại bỏ 

[ 
x=\min(a_s,\lfloor n/k_s\rfloor) 
] 

người chơi từ đội rẻ hơn. Các thẻ còn lại là (n-xk_s) nên chúng ta có thể loại bỏ 

[ 
\min(a_l,\lfloor(n-xk_s)/k_l\rfloor) 
] 

các cầu thủ của đội khác. 

Sự lựa chọn tham lam này là tối ưu vì việc thay thế loại bỏ rẻ bằng một loại đắt tiền không bao giờ làm tăng số lần loại bỏ có thể đạt được từ một số lượng thẻ cố định. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Nhiệm vụ đầy đủ | (O((a_1+a_2)^n)) | (O(n)) độ sâu đệ quy | Quá chậm | 
| Liệt kê sự chia tách đội | (O(n)) | (O(1)) | Được chấp nhận nhưng không cần thiết | 
| Số học tham lam | (O(1)) | (O(1)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính số thẻ có thể chia cho mỗi người chơi mà không loại bỏ ai: 

[ 
an toàn=a_1(k_1-1)+a_2(k_2-1). 
] 

Số lần xóa tối thiểu là`max(0, n - safe)`. Mỗi thẻ vượt quá`safe`phải hoàn thành ngưỡng của một người chơi an toàn trước đó và một ngưỡng đã hoàn thành sẽ loại bỏ chính xác một người chơi. 
2. So sánh (k_1) và (k_2) và coi đội có ngưỡng nhỏ hơn là đội đầu tiên có phép tính tối đa. 

Việc xóa khỏi nhóm này sẽ tiêu tốn ít thẻ hơn, vì vậy việc sử dụng thẻ này trước tiên sẽ mang lại số lần xóa tốt nhất có thể. 
3. Loại bỏ càng nhiều người chơi càng tốt khỏi đội có ngưỡng nhỏ hơn: 

[ 
x=\min(a_s,n//k_s). 
] 

các`min`là cần thiết vì chỉ có (a_s) người chơi trong đội đó. 
4. Trừ đi số thẻ (xk_s) được sử dụng bởi những người chơi bị loại bỏ đó. 

Các thẻ còn lại có thể được sử dụng để loại bỏ người chơi khỏi đội khác. Bất kỳ số tiền còn lại nào dưới ngưỡng của nó đều có thể được chỉ định cho một người chơi mà không cần xóa người chơi đó. 
5. Loại bỏ càng nhiều người chơi khỏi đội khác càng tốt: 

[ 
y=\min(a_l,(n-xk_s)//k_l). 
] 

Câu trả lời tối đa là (x+y). 

### Tại sao nó hoạt động 

Ở mức tối thiểu, mọi người chơi có thể hấp thụ chính xác (k_i-1) thẻ khi ở trong trò chơi. Điều này mang lại tổng công suất an toàn là (an toàn). Khi những lá bài đó đã được sử dụng, mọi lá bài bổ sung phải là lá bài thứ (k_i) của một số người chơi và do đó sẽ bị loại bỏ. Vì đầu vào đảm bảo (n) không vượt quá tổng dung lượng (a_1k_1+a_2k_2) nên luôn có đủ người chơi để hấp thụ hết quân bài thừa. 

Để đạt mức tối đa, hãy xem xét bất kỳ nhóm người chơi nào đã bị loại bỏ. Mỗi lần loại bỏ đội thứ nhất tốn (k_1) thẻ và mỗi lần loại bỏ đội thứ hai tốn (k_2) thẻ. Nếu (k_1<k_2), việc thay thế một lần loại bỏ của nhóm thứ hai bằng một lần loại bỏ của đội thứ nhất không bao giờ yêu cầu nhiều thẻ hơn và chỉ có thể để lại ít nhất số thẻ có sẵn cho những lần loại bỏ tiếp theo. Do đó, một sự sắp xếp tối ưu luôn có thể được chuyển đổi thành một sự sắp xếp sử dụng càng nhiều lần loại bỏ ngưỡng nhỏ càng tốt trước khi loại bỏ ngưỡng lớn hơn. Lập luận tương tự áp dụng với các đội đảo ngược khi (k_2<k_1). Sau khi tất cả các lần loại bỏ đã chọn đã được thanh toán, những lá bài chưa sử dụng có thể được đặt giữa những người chơi chưa đạt đến ngưỡng của họ, do đó có thể đạt được số lượng tham lam. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    a1 = int(input())
    a2 = int(input())
    k1 = int(input())
    k2 = int(input())
    n = int(input())

    safe = a1 * (k1 - 1) + a2 * (k2 - 1)
    minimum = max(0, n - safe)

    if k1 <= k2:
        small_count, small_k = a1, k1
        large_count, large_k = a2, k2
    else:
        small_count, small_k = a2, k2
        large_count, large_k = a1, k1

    removed_small = min(small_count, n // small_k)
    remaining = n - removed_small * small_k

    removed_large = min(large_count, remaining // large_k)
    maximum = removed_small + removed_large

    print(minimum, maximum)

if __name__ == "__main__":
    solve()
```Năm lệnh gọi đầu vào đầu tiên đọc hai quy mô đội, ngưỡng loại bỏ tương ứng của họ và tổng số thẻ vàng. Không có nhiều trường hợp thử nghiệm trong vấn đề này, vì vậy hãy gọi tới`solve()`là đủ. 

các`safe`cách sử dụng biểu thức`k_i - 1`, không`k_i`. Việc tiếp cận chính xác các thẻ (k_i) sẽ loại bỏ người chơi, vì vậy chỉ có các thẻ (k_i-1) có sẵn khi chúng tôi yêu cầu không ai bị loại bỏ. 

Ở mức tối đa, mã hoán đổi vai trò khái niệm của các nhóm thay vì sửa đổi vật lý các giá trị đầu vào. Sau khi so sánh,`small_k`được đảm bảo không lớn hơn`large_k`. Phép chia số nguyên cho các ngưỡng này sẽ cho ra số lượng nhóm thẻ hoàn chỉnh có thể tạo ra sự loại bỏ. 

các`min`so với quy mô của đội sẽ ngăn cản việc tính số lần loại bỏ nhiều hơn số người chơi. Sau khi thanh toán cho những lần loại bỏ đó, số thẻ còn lại được chia cho ngưỡng lớn hơn để tìm xem có bao nhiêu người chơi bổ sung có thể rời đi. 

Số nguyên Python có độ chính xác tùy ý, do đó không có vấn đề tràn. Ngay cả các số nguyên 32 bit có chiều rộng cố định ở đây cũng đủ vì sản phẩm lớn nhất chỉ có (10^6) cho mỗi nhóm, nhưng việc sử dụng số nguyên Python sẽ loại bỏ hoàn toàn mối lo ngại. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
2
3
5
1
8
```Đội đầu tiên có hai người chơi, mỗi người cần năm thẻ, trong khi mỗi người trong số ba người chơi ở đội thứ hai chỉ cần một thẻ. 

| Biến | Giá trị | 
| --- | --- | 
| (a_1) | 2 | 
| (a_2) | 3 | 
| (k_1) | 5 | 
| (k_2) | 1 | 
| (n) | 8 | 
| Năng lực an toàn | (2(5-1)+3(1-1)=8) | 
| Tối thiểu | 0 | 
| Ngưỡng nhỏ hơn | 1 | 
| Loại bỏ nhóm nhỏ | (\min(3,8//1)=3) | 
| Thẻ còn lại | (8-3=5) | 
| Loại bỏ đội khác | (\min(2,5//5)=1) | 
| Tối đa | 4 | 

Dung lượng an toàn chính xác là tám, vì vậy tất cả các thẻ có thể được phân phối mà không cần loại bỏ. Tối đa, ba người chơi ở đội thứ hai có thể nhận được một thẻ yêu cầu duy nhất của mình và năm thẻ còn lại sẽ loại bỏ một cầu thủ ở đội một. Đầu ra là`0 4`. 

### Mẫu 2 

Đầu vào là```
3
1
6
7
25
```Tổng dung lượng thẻ có thể có chính xác là (3\cdot6+1\cdot7=25). 

| Biến | Giá trị | 
| --- | --- | 
| (a_1) | 3 | 
| (a_2) | 1 | 
| (k_1) | 6 | 
| (k_2) | 7 | 
| (n) | 25 | 
| Năng lực an toàn | (3(6-1)+1(7-1)=21) | 
| Tối thiểu | (25-21=4) | 
| Ngưỡng nhỏ hơn | 6 | 
| Loại bỏ nhóm nhỏ | (\min(3,25//6)=3) | 
| Thẻ còn lại | (25-18=7) | 
| Loại bỏ đội khác | (\min(1,7//7)=1) | 
| Tối đa | 4 | 

Số lượng thẻ tối đa đã được hiển thị nên mọi người chơi phải nhận được chính xác ngưỡng loại bỏ của mình. Cả tối thiểu và tối đa là bốn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(1)) | Chỉ một số lượng phép tính và so sánh số học không đổi được thực hiện | 
| Không gian | (O(1)) | Thuật toán chỉ lưu trữ một số số nguyên cố định | 

Giá trị đầu vào lớn nhất tạo ra sản phẩm chỉ khoảng (10^6) cho mỗi đội và tổng số tối đa (2\cdot10^6) thẻ. Giải pháp không lặp lại các thẻ hoặc người chơi, vì vậy nó thoải mái trong giới hạn thời gian một giây và sử dụng bộ nhớ không đáng kể. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_io(inp: str) -> str:
    data = list(map(int, inp.split()))
    a1, a2, k1, k2, n = data

    safe = a1 * (k1 - 1) + a2 * (k2 - 1)
    minimum = max(0, n - safe)

    if k1 <= k2:
        small_count, small_k = a1, k1
        large_count, large_k = a2, k2
    else:
        small_count, small_k = a2, k2
        large_count, large_k = a1, k1

    removed_small = min(small_count, n // small_k)
    remaining = n - removed_small * small_k
    removed_large = min(large_count, remaining // large_k)

    maximum = removed_small + removed_large

    return f"{minimum} {maximum}\n"

# Provided samples
assert solve_io("""2
3
5
1
8
""") == "0 4\n", "sample 1"

assert solve_io("""3
1
6
7
25
""") == "4 4\n", "sample 2"

assert solve_io("""6
4
9
10
89
""") == "5 9\n", "sample 3"

# Minimum-size input
assert solve_io("""1
1
1
1
1
""") == "1 1\n", "single card with threshold 1"

# All thresholds equal and maximum possible number of cards
assert solve_io("""1000
1000
1000
1000
2000000
""") == "2000 2000\n", "maximum-size input"

# Boundary where nobody has to leave
assert solve_io("""1
1
2
3
3
""") == "0 1\n", "exact safe capacity"

# Boundary just above safe capacity
assert solve_io("""2
1
2
3
4
""") == "1 2\n", "one card above safe capacity"

# Different thresholds, smaller threshold belongs to team 2
assert solve_io("""2
3
5
2
9
""") == "0 4\n", "greedy must use team 2 first"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1 1 1`|`1 1`| Trường hợp và ngưỡng nhỏ nhất có thể (1) | 
|`1000 1000 1000 1000 2000000`|`2000 2000`| Kích thước đầu vào tối đa và tất cả người chơi đã bị xóa | 
|`1 1 2 3 3`|`0 1`| Ranh giới công suất an toàn chính xác | 
|`2 1 2 3 4`|`1 2`| Một thẻ vượt quá dung lượng an toàn và ranh giới loại bỏ tối đa | 
|`2 3 5 2 9`|`0 4`| Ngưỡng nhỏ hơn thuộc về đội thứ hai | 

## Vỏ cạnh 

Đối với ranh giới công suất an toàn chính xác,```
1
1
2
3
3
```người chơi đầu tiên có thể nhận được một lá bài một cách an toàn và người thứ hai có thể nhận được hai lá bài một cách an toàn. Như vậy`safe = 1 + 2 = 3`, cho ít nhất`max(0, 3 - 3) = 0`. Tối đa, người chơi thứ hai có thể nhận được cả ba lá bài và rời đi, vì vậy câu trả lời là`0 1`. Thuật toán không coi quân bài thứ ba là lá bài buộc phải loại bỏ khỏi người chơi đầu tiên. 

Đối với ngưỡng một,```
1
1
1
5
1
```người chơi đầu tiên có năng lực an toàn (1-1=0). Thẻ đơn ngay lập tức đạt đến ngưỡng của chúng, vì vậy mức tối thiểu là một. Thẻ tương tự cũng đủ cho mức tối đa, cho`1 1`. Phép trừ một trong phép tính công suất an toàn xử lý chính xác ranh giới này. 

Đối với trường hợp ngưỡng nhỏ hơn thuộc về đội thứ hai,```
2
3
5
2
9
```dung lượng an toàn là (2(5-1)+3(2-1)=11), do đó, chín thẻ có thể được phân phát mà không cần loại bỏ và mức tối thiểu là bằng không. Để tối đa hóa số lần loại bỏ, thuật toán sẽ xử lý nhóm ngưỡng hai trước tiên. Nó có thể loại bỏ cả ba người chơi đó bằng sáu lá bài, để lại ba lá bài. Ba lá bài đó không thể loại bỏ người chơi ở ngưỡng năm, vì vậy mức tối đa thực tế sẽ là ba chứ không phải bốn. 

Ví dụ này cho thấy lý do tại sao bài kiểm tra trên phải sử dụng số học chính xác. Khẳng định tương ứng được cố ý sửa dưới đây:```
assert solve_io("""2
3
5
2
9
""") == "0 3\n", "greedy must use team 2 first"
```Để có được đầu vào tối đa có thể,```
1000
1000
1000
1000
2000000
```mỗi người trong số (2000) người chơi cần (1000) thẻ và chính xác (2.000.000) thẻ có sẵn. Tính toán tối thiểu cho 

[ 
2.000.000-2.000(999)=2.000, 
] 

trong khi phép tính tối đa loại bỏ tất cả (2000) người chơi. Do đó đầu ra là`2000 2000`. 

Đối với trường hợp ngay trên mức an toàn,```
2
1
2
3
4
```công suất an toàn là (2(1)+1(2)=4), vì vậy mức tối thiểu thực sự là 0, không phải một. Đây là một bước kiểm tra ranh giới hữu ích khác vì nó phát hiện từng lỗi một trong định nghĩa về năng lực an toàn. Đầu ra chính xác cho đầu vào này là`0 2`: Hai người chơi ở đội một mỗi người được nhận hai lá bài, loại bỏ cả hai, trong khi tổng số còn lại đúng là bốn lá bài. Bài kiểm tra tương ứng phải là:```
assert solve_io("""2
1
2
3
4
""") == "0 2\n", "exact safe capacity with different thresholds"
```Các trường hợp ranh giới được hiệu chỉnh này củng cố sự khác biệt trung tâm của bài toán: thẻ (k_i-1) là an toàn, trong khi thẻ thứ (k_i) gây ra sự loại bỏ. Việc duy trì sự khác biệt rõ ràng sẽ ngăn ngừa các lỗi thường gặp nhất ở cả hai phần của giải pháp.
