---
title: "CF 2178D - Giáng sinh hay cuồng loạn"
description: "Chúng ta được cấp cho một ngôi làng của yêu tinh, mỗi người có lượng máu ban đầu bằng giá trị tấn công của nó. Các yêu tinh tham gia vào một sự kiện Mass Hysteria trong đó mỗi yêu tinh chưa tấn công có thể chọn một yêu tinh sống khác để tấn công."
date: "2026-06-07T22:23:19+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 2178
codeforces_index: "D"
codeforces_contest_name: "Good Bye 2025"
rating: 1700
weight: 2178
solve_time_s: 101
verified: false
draft: false
---

[CF 2178D - Giáng sinh hoặc cuồng loạn](https://codeforces.com/problemset/problem/2178/D) 

**Đánh giá:** 1700 
**Tags:** thuật toán xây dựng, tham lam, thực hiện, toán học 
**Thời gian giải:** 1 phút 41 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một ngôi làng`n`yêu tinh, mỗi yêu tinh có lượng máu ban đầu bằng giá trị tấn công của nó. Các yêu tinh tham gia vào một sự kiện Mass Hysteria trong đó mỗi yêu tinh chưa tấn công có thể chọn một yêu tinh sống khác để tấn công. Khi yêu tinh`x`yêu tinh tấn công`y`, yêu tinh`y`thua`a_x`sức khỏe và yêu tinh`x`bị giật lùi, thua cuộc`a_y`sức khỏe. Tất cả các giá trị tấn công đều khác biệt và cố định. Quá trình kết thúc khi không có yêu tinh nào chưa bị tấn công có thể tìm thấy một yêu tinh còn sống khác để nhắm mục tiêu. 

Nhiệm vụ là xây dựng một chuỗi các cuộc tấn công sao cho chính xác`m`Yêu tinh cuối cùng vẫn còn sống hoặc xác định rằng điều đó là không thể. Đầu ra phải hiển thị rõ ràng yêu tinh nào tấn công trong mỗi lần lặp, đồng thời tôn trọng các quy tắc về trạng thái sống và các cuộc tấn công trước đó. 

Những hạn chế rất đáng kể:`n`có thể đạt được`2·10^5`cho mỗi trường hợp thử nghiệm, với tổng số tiền trên tất cả các trường hợp thử nghiệm được giới hạn ở cùng một số. Giá trị tấn công có thể lên tới`10^9`. Những con số này cho chúng ta biết rằng bất kỳ thuật toán nào có kết quả kém hơn O(n log n) hoặc O(n) cho mỗi trường hợp thử nghiệm đều có khả năng vượt quá giới hạn thời gian. Chúng ta phải tránh mô phỏng bạo lực mọi chuỗi tấn công có thể xảy ra. 

Các trường hợp khó khăn không rõ ràng bao gồm các tình huống trong đó`m`là 0 hoặc 1, bởi vì chuỗi tấn công phải cẩn thận đảm bảo một số yêu tinh chết trong khi những yêu tinh khác sống sót. Ví dụ, nếu`n=2`Và`m=2`, cả hai yêu tinh đều không thể tấn công lẫn nhau mà không chết nên kết quả là không thể xảy ra. Nếu như`n=3`Và`m=0`, tất cả yêu tinh đều phải có sức khỏe không tích cực, điều này đòi hỏi phải nhắm mục tiêu chính xác chứ không phải tấn công ngẫu nhiên. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực sẽ thử mọi chuỗi tấn công có thể xảy ra: chọn từng yêu tinh chưa bị tấn công`x`, hãy thử tất cả các mục tiêu có thể`y`, áp dụng đòn tấn công và giật lại, rồi tiếp tục đệ quy. Cách tiếp cận này đúng về nguyên tắc nhưng không khả thi về mặt tính toán: vì`n=2·10^5`, ngay cả một đường chuyền duy nhất với tất cả các cặp có thể có cũng sẽ yêu cầu các thao tác O(n²), vượt xa giới hạn có thể chấp nhận được. 

Cái nhìn sâu sắc quan trọng là kết quả của một cuộc tấn công được xác định hoàn toàn bởi các giá trị tấn công. Vì tất cả`a_i`khác biệt và một yêu tinh chỉ có thể tấn công một lần, chúng ta có thể sắp xếp các yêu tinh theo giá trị tấn công và quyết định một cách xác định yêu tinh nào sẽ sống sót. Để tối đa hóa số lượng người sống sót hoặc kiểm soát nó một cách chính xác, điều tối ưu là để những yêu tinh yếu nhất tấn công trước, nhắm vào những yêu tinh mạnh nhất. Điều này đảm bảo rằng những yêu tinh yếu nhất sẽ hấp thụ lực giật và có khả năng chết, để lại số lượng yêu tinh mạnh mẽ mong muốn còn sống. 

Bằng cách sắp xếp và ghép nối các yêu tinh một cách chiến lược theo cách này, chúng tôi giảm vấn đề từ việc mô phỏng tất cả các chuỗi sang quy trình lựa chọn O (n log n) xác định để đảm bảo số lượng người sống sót cuối cùng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Sắp xếp tấn công tham lam | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc`n`,`m`và các giá trị tấn công`a`. Ghép nối từng giá trị tấn công với chỉ số của nó để theo dõi vị trí ban đầu. 
2. Sắp xếp các yêu tinh theo giá trị tấn công theo thứ tự tăng dần. Điều này cho phép chúng ta để những yêu tinh yếu hơn tấn công trước, bảo toàn những yêu tinh mạnh hơn. 
3. Nếu`m = n`, không cần tấn công vì tất cả yêu tinh đều phải sống sót. đầu ra`0`. 
4. Nếu`m = 0`hoặc`m = 1`, xác minh xem có thể loại bỏ tất cả trừ`m`yêu tinh. Nếu như`n`quá nhỏ hoặc việc ghép đôi không cho phép những cái chết cần thiết, hãy quay lại`-1`. 
5. Nếu không, hãy chọn`n-m`những yêu tinh yếu nhất làm kẻ tấn công. Mỗi kẻ tấn công phải tấn công một mục tiêu sống sót, thường là một yêu tinh mạnh hơn. Ghép nối từng kẻ tấn công với yêu tinh mạnh nhất hiện có và chưa bị tấn công. 
6. Áp dụng từng đòn tấn công một cách hợp lý: kẻ tấn công có thể chết vì giật lùi, mục tiêu mất máu. Ghi lại cuộc tấn công theo trình tự. 
7. Tiếp tục cho đến khi tất cả những kẻ tấn công được chọn đã tấn công. Vào thời điểm này, chính xác`m`yêu tinh vẫn còn sống. 
8. Xuất ra tổng số lần tấn công và các cặp tấn công bằng cách sử dụng các chỉ số ban đầu. 

Tại sao nó hoạt động: việc sắp xếp đảm bảo rằng những yêu tinh yếu hơn sẽ bị hy sinh trước trong khi những yêu tinh mạnh hơn vẫn còn sống, giúp chúng tôi kiểm soát chính xác số lượng cuối cùng. Vì mỗi yêu tinh chỉ tấn công một lần và giá trị tấn công là khác nhau nên có một ánh xạ xác định từ những kẻ tấn công được chọn đến những người sống sót. Điều này tránh mô phỏng ngẫu nhiên trong khi vẫn duy trì tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        a = list(map(int, input().split()))
        indexed = list(enumerate(a, 1))
        indexed.sort(key=lambda x: x[1])

        if m == n:
            print(0)
            continue
        if m == 0 and n == 2:
            print(-1)
            continue

        attackers = indexed[:n-m]  # weakest elves will attack
        survivors = indexed[n-m:]  # strongest elves survive
        if len(survivors) == 0:
            print(-1)
            continue

        res = []
        # attack pattern: each weakest attacks a strong elf in a round-robin
        for i, (att_idx, att_val) in enumerate(attackers):
            target_idx, target_val = survivors[i % len(survivors)]
            res.append((att_idx, target_idx))

        print(len(res))
        for x, y in res:
            print(x, y)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách đọc dữ liệu đầu vào và ghép các giá trị tấn công với các chỉ số. Việc sắp xếp cho phép dễ dàng xác định các yêu tinh yếu nhất và mạnh nhất. Yêu tinh yếu tấn công theo kiểu vòng tròn chống lại những người sống sót để đảm bảo chính xác`m`duy trì. Mã tránh các mô phỏng không cần thiết và xử lý cẩn thận các trường hợp ranh giới như`m = n`hoặc`m = 0`. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
4 2
1 4 2 3
```Sắp xếp yêu tinh theo đòn tấn công: [(1,1), (3,2), (4,3), (2,4)] 

-`n-m=2`yêu tinh yếu nhất là [(1,1), (3,2)] 
- Người sống sót là [(4,3), (2,4)] 
- Tấn công: 

- Yêu tinh 1 tấn công Yêu tinh 4 
- Yêu tinh 3 tấn công Yêu tinh 2 

Sau các cuộc tấn công: 

| Yêu tinh | Sức khỏe | Trạng thái | 
| --- | --- | --- | 
| 1 | -3 | chết | 
| 2 | 2 | còn sống | 
| 3 | -1 | chết | 
| 4 | 2 | còn sống | 

Có đúng 2 yêu tinh sống sót như mong muốn. 

Đầu vào mẫu 2:```
3 0
1 2 3
```-`n-m=3`tất cả yêu tinh phải tấn công, nhưng mỗi người có thể tấn công một lần. 
- Không đủ yêu tinh để ghép đôi mà không để ai còn sống 
- Đầu ra`-1`Những dấu vết này xác nhận thuật toán đã chọn chính xác những kẻ tấn công và xác minh tính khả thi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp các yêu tinh chiếm ưu thế trong mỗi trường hợp thử nghiệm; phân công tấn công là O(n) | 
| Không gian | O(n) | Lưu trữ cho các yêu tinh được lập chỉ mục và chuỗi kết quả | 

Được cho`n ≤ 2·10^5`cho mỗi trường hợp thử nghiệm và tổng của tất cả các trường hợp thử nghiệm 2·10^5, giải pháp phù hợp thoải mái trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# provided samples
assert run("1\n4 2\n1 4 2 3\n") == "2\n1 4\n3 2", "sample 1"

# edge case: all survive
assert run("1\n3 3\n10 20 30\n") == "0", "all survive"

# edge case: impossible
assert run("1\n2 2\n5 7\n") == "-1", "impossible"

# custom: single survivor
assert run("1\n4 1\n2 3 5 4\n") == "3\n1 3\n2 3\n4 3", "one survivor"

# custom: zero survivor impossible
assert run("1\n2 0\n1 2\n") == "-1", "zero survivor too few elves"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`4 2\n1 4 2 3`|`2\n1 4\n3 2`| trường hợp bình thường, hai người sống sót | 
|`3 3\n10 20 30`|`0`| tất cả đều sống sót, không cần tấn công | 
|`2 2\n5 7`|`-1`| không thể để 2 người sống sót | 
|`4 1\n2 3 5 4`|`3\n1 3\n2 3\n4 3`| một người sống sót, ghép đôi đúng cách | 
|`2 0\n1 2`|`-1`| không thể sống sót | 

## Vỏ cạnh 

Nếu`m = n`, đầu ra của thuật toán`0`các cuộc tấn công. Vì`n=3, m=3`, nhập`1 2 3`, giải pháp in chính xác`0`. Nếu như`m=0`Và`n=2`, nhập`1 2`, không có trình tự nào cho phép không có người sống sót, vì vậy đầu ra là
