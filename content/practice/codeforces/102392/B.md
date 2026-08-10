---
title: "CF 102392B - Tăng cấp"
description: "Steve có một bộ nhiệm vụ và mỗi nhiệm vụ chỉ có thể hoàn thành tối đa một lần. Trước khi hoàn thành cấp độ đầu tiên, nhiệm vụ (i) mang lại (xi) kinh nghiệm và tiêu tốn (ti) phút. Sau khi hoàn thành cấp độ đầu tiên, nhiệm vụ tương tự chỉ mang lại (yi) kinh nghiệm và tiêu tốn (ri) phút."
date: "2026-08-10T19:26:08+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102392
codeforces_index: "B"
codeforces_contest_name: "2019-2020 ICPC Southeastern European Regional Programming Contest (SEERC 2019)"
rating: 0
weight: 102392
solve_time_s: 137
verified: true
draft: false
---

[CF 102392B - Tăng cấp](https://codeforces.com/problemset/problem/102392/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 17s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Steve có một bộ nhiệm vụ và mỗi nhiệm vụ chỉ có thể hoàn thành tối đa một lần. Trước khi hoàn thành cấp độ đầu tiên, nhiệm vụ (i) mang lại (x_i) kinh nghiệm và chi phí (t_i) phút. Sau khi hoàn thành cấp độ đầu tiên, nhiệm vụ tương tự chỉ mang lại (y_i) kinh nghiệm và chi phí (r_i) phút. 

Cấp độ đầu tiên yêu cầu (s_1) kinh nghiệm. Khi Steve đạt được nó, bất kỳ trải nghiệm nào vượt quá (s_1) từ nhiệm vụ khiến bạn thăng cấp sẽ ngay lập tức được tính vào cấp độ thứ hai. Cấp độ thứ hai sau đó yêu cầu (s_2) trải nghiệm khác, bị giảm do lượng tràn đó. Mục tiêu là chọn nhiệm vụ nào được thực hiện trước khi lên cấp và nhiệm vụ nào được thực hiện sau đó, cùng với thứ tự hoàn thành hai cấp độ trong tổng thời gian tối thiểu. Nếu không có lựa chọn nào như vậy thì câu trả lời là (-1). Trang vấn đề chính thức xác nhận rằng các nhiệm vụ không thể lặp lại và việc tràn từ cấp độ đầu tiên sẽ chuyển trực tiếp sang cấp độ thứ hai. 

Yêu cầu cấp một và cấp hai nhiều nhất là 500, trong khi mọi giá trị kinh nghiệm cũng nhiều nhất là 500. Đó là tín hiệu cho thấy một chương trình năng động được lập chỉ mục theo kinh nghiệm tích lũy có thể thực tế. Số lượng nhiệm vụ nhiều nhất là 500, do đó thuật toán (O(n s_1 s_2)) thực hiện khoảng (125,5) triệu lần lặp trạng thái trong trường hợp xấu nhất tuyệt đối. Một giải pháp liệt kê mọi nhiệm vụ có thể được phân công có (3^n), khả năng này đã nằm trong khoảng (10^{238}) cho (n=500), vì vậy việc sử dụng vũ lực hoàn toàn không còn cần thiết nữa. Bài xã luận chính thức đưa ra ràng buộc lập trình động (O(n s_1 s_2)) tương tự. 

Có một số trường hợp việc thực hiện bất cẩn có thể tạo ra câu trả lời hợp lý nhưng không chính xác. Đầu tiên là tràn. Coi như```
2 5 5
6 10 1 1
1 1 5 1
```Câu trả lời đúng là`11`. Nhiệm vụ đầu tiên phải được hoàn thành trước khi lên cấp, mang lại 6 điểm kinh nghiệm. Cấp độ đầu tiên chỉ cần 5, vì vậy 1 kinh nghiệm sẽ tràn vào cấp độ thứ hai. Nhiệm vụ thứ hai sẽ mang lại thêm 5 kinh nghiệm sau khi lên cấp, vì vậy cấp độ thứ hai nhận được tổng cộng 6 kinh nghiệm. Một DP chỉ giới hạn kinh nghiệm đầu tiên ở (s_1) và loại bỏ phần vượt quá sẽ kết luận sai rằng cấp độ thứ hai chỉ nhận được 5 kinh nghiệm. 

Trường hợp thứ hai là nhiệm vụ được DP xem xét sớm không nhất thiết phải được hoàn thành về mặt vật lý trước khi lên cấp. Ví dụ,```
2 100 100
100 100 10 10
101 11 100 10
```có câu trả lời`110`. Nhiệm vụ thứ hai rẻ hơn ở cấp độ đầu tiên, trong khi nhiệm vụ đầu tiên rẻ hơn ở cấp độ thứ hai, nhưng các nhiệm vụ phải được sắp xếp sao cho thực sự đạt đến cấp độ đầu tiên trước khi sử dụng phiên bản cấp độ thứ hai của nhiệm vụ. Một DP coi thứ tự đầu vào là thứ tự thực hiện thực tế có thể bỏ lỡ nhiệm vụ tối ưu. 

Trường hợp cạnh thứ ba là không thể. Vì```
2 20 5
10 10 5 5
10 10 5 5
```câu trả lời là`-1`. Cả hai nhiệm vụ cùng nhau chỉ cung cấp 20 kinh nghiệm cấp độ một, chính xác là đủ để đạt đến cấp độ đầu tiên, nhưng sau khi tiêu thụ cả hai nhiệm vụ thì không còn gì cho cấp độ thứ hai. Một giải pháp bất cẩn chỉ kiểm tra xem liệu cấp độ đầu tiên có thể được hoàn thành hay không sẽ chấp nhận trường hợp này một cách sai lầm. 

## Phương pháp tiếp cận 

Lực lượng vũ phu trực tiếp nhất giao cho mỗi nhiệm vụ một trong ba vai trò: không được sử dụng, hoàn thành trước cấp độ đầu tiên hoặc hoàn thành sau cấp độ đầu tiên. Đối với mỗi nhiệm vụ trong số (3^n), chúng tôi có thể kiểm tra xem nhiệm vụ cấp độ đầu tiên có thể đạt được (s_1) hay không, tính toán mức tràn và sau đó kiểm tra xem nhiệm vụ cấp độ thứ hai có cung cấp lượng kinh nghiệm còn lại hay không. Chúng ta cũng có thể tìm kiếm trực tiếp qua các thứ tự có thể có, nhưng điều đó thậm chí còn tệ hơn vì nó đưa ra các hoán vị bên trên các lựa chọn bài tập. Chỉ riêng phép gán ba chiều đã mang lại (3^{500}) khả năng, do đó phương pháp này gần như không thể sử dụng được ngay lập tức. 

Bước tiếp theo tự nhiên là lập trình động. Giả sử chúng ta xử lý các nhiệm vụ theo một số thứ tự cố định và giữ (j) là trải nghiệm cấp một và (k) là trải nghiệm cấp hai. Đối với mỗi nhiệm vụ, chúng ta có thể bỏ qua nó, gán nó cho cấp độ đầu tiên hoặc gán nó cho cấp độ thứ hai. Nếu nhiệm vụ cấp một đẩy (j) qua (s_1), phần vượt quá của nó phải được cộng vào (k). Điều này đưa ra trạng thái chỉ (O(s_1s_2)), thay vì ghi nhớ những nhiệm vụ riêng lẻ nào đã được chọn. 

Có một trở ngại: các nhiệm vụ không có thứ tự thực hiện cố định. Cụ thể, nhiệm vụ nào khiến cấp độ đầu tiên kết thúc rất quan trọng vì nhiệm vụ đó quyết định mức tràn. 

Quan sát quan trọng là chúng ta có thể sắp xếp các nhiệm vụ bằng cách tăng dần (x_i). Hãy xem xét bất kỳ giải pháp hợp lệ nào và xem xét nhiệm vụ thực sự khiến cấp độ đầu tiên kết thúc. Giả sử trải nghiệm cấp độ đầu tiên của nó là (U). Tất cả các nhiệm vụ khác được giao ở cấp độ đầu tiên đều đã được hoàn thành trước đó và tổng kinh nghiệm của họ vẫn ở mức dưới (s_1). Nếu một nhiệm vụ cấp độ đầu tiên khác trong cùng một giải pháp cho ra (V \ge U), thay vào đó chúng ta có thể chuyển nhiệm vụ đó đến vị trí cuối cùng. Tổng thời gian và nhóm nhiệm vụ được sử dụng không thay đổi, trong khi giá trị kinh nghiệm lớn hơn ít nhất cũng có khả năng giúp tăng cấp. Việc lặp lại lập luận này cho phép chúng ta chọn nhiệm vụ cấp đầu tiên lớn nhất-(x_i) làm nhiệm vụ gây ra tình trạng tràn. Do đó, sau khi sắp xếp theo (x_i), sẽ có một giải pháp tối ưu có nhiệm vụ cấp một được xử lý theo thứ tự được sắp xếp này. Đây chính xác là quan sát thứ tự được sử dụng trong bài xã luận chính thức. 

Các nhiệm vụ cấp hai xuất hiện trước đó trong quá trình xử lý được sắp xếp này chỉ được DP chọn. Chúng chưa cần phải được hoàn thiện về mặt vật lý. Tất cả đều có thể bị hoãn lại cho đến khi đạt được cấp độ đầu tiên. Đây là lý do tại sao DP có thể tích lũy kinh nghiệm cấp hai một cách an toàn ngay cả khi (j<s_1). 

Với các nhiệm vụ được sắp xếp, DP trở nên đơn giản hơn. Trạng thái lưu trữ thời gian tối thiểu cần thiết để đạt được chính xác số lượng giới hạn được thể hiện của tiến độ cấp một và cấp hai. Bỏ qua một nhiệm vụ sẽ khiến trạng thái không thay đổi. Lấy nó ở cấp độ thứ hai tăng (k) lên (y_i). Lấy nó ở cấp độ đầu tiên sẽ tăng (j) lên (x_i) và nếu vượt qua (s_1), phần vượt quá sẽ được thêm vào (k). 

DP có thể được thực hiện tại chỗ. Chúng tôi xử lý cả hai thứ nguyên theo thứ tự giảm dần, do đó, mọi chuyển đổi sẽ chuyển sang trạng thái đã được xử lý cho nhiệm vụ hiện tại. Do đó, nhiệm vụ hiện tại không thể được sử dụng hai lần. Thao tác này sẽ loại bỏ mảng tạm thời (s_1 \times s_2) bổ sung được sử dụng bởi quá trình triển khai đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(3^n)) | (O(n)) | Quá chậm | 
| DP tối ưu | (O(n s_1 s_2)) | (O(s_1 s_2)) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Đọc tất cả các nhiệm vụ và sắp xếp chúng theo thứ tự tăng dần (x_i). Việc sắp xếp mang lại cho chúng ta một thứ tự chuẩn mực trong đó các nhiệm vụ cấp độ đầu tiên có thể được xem xét. Một giải pháp tối ưu luôn có thể được biểu diễn bằng cách sử dụng thứ tự này vì nhiệm vụ khiến cấp độ đầu tiên kết thúc có thể được chọn làm nhiệm vụ tối đa (x_i) trong số các nhiệm vụ được giao cho cấp độ đầu tiên. 
2. Tạo`dp[j][k]`, Ở đâu`j`là kinh nghiệm tích lũy ở cấp độ đầu tiên được giới hạn ở (s_1),`k`là kinh nghiệm cấp hai tích lũy được giới hạn ở (s_2) và giá trị được lưu trữ là số phút tối thiểu cần thiết để đạt đến trạng thái đó bằng cách sử dụng các nhiệm vụ được xử lý cho đến nay. Khởi tạo mọi trạng thái đến vô cùng ngoại trừ`dp[0][0] = 0`. 
3. Đối với nhiệm vụ hiện tại, hãy xem xét mọi trạng thái có thể tiếp cận được. Khả năng đầu tiên là bỏ qua nhiệm vụ. Vì DP được thực hiện tại chỗ nên việc bỏ qua không yêu cầu gán gì cả và không cần thao tác rõ ràng. 
4. Khả năng thứ hai là sử dụng nhiệm vụ sau cấp độ đầu tiên. Trạng thái của nó trở thành 
[ 
(j,\min(s_2,k+y_i)) 
] 
và giá của nó tăng thêm (r_i). Quá trình chuyển đổi này được cho phép ngay cả khi (j<s_1), vì DP đang chọn nhiệm vụ để sử dụng sau. Việc thực thi vật lý của nó có thể bị hoãn lại cho đến khi cấp độ đầu tiên hoàn thành. 
5. Khả năng thứ ba là sử dụng nhiệm vụ trước cấp độ đầu tiên. Tính (j+x_i). Nếu giá trị này ở dưới (s_1), thì trạng thái mới chỉ đơn giản là ((j+x_i,k)), với chi phí tăng thêm (t_i). Nếu nó đạt hoặc vượt quá (s_1), trạng thái cấp một sẽ trở thành (s_1) và phần vượt quá 
[ 
j+x_i-s_1 
] 
được thêm vào tiến trình cấp độ thứ hai. Như vậy trạng thái mới là 
[ 
\left(s_1,\min(s_2,k+j+x_i-s_1)\right). 
] 
Lượng tràn phải được giữ lại vì nó trực tiếp làm giảm lượng kinh nghiệm còn cần cho cấp độ thứ hai. 
6. Quy trình`j`từ (s_1) xuống 0 và`k`từ (s_2) xuống 0. Mọi chuyển đổi không bỏ qua đều tăng`j`hoặc`k`, do đó, lần lặp giảm dần có nghĩa là đích đến của nó đã được truy cập cho nhiệm vụ hiện tại. Do đó, nhiệm vụ không thể đóng góp hai lần cho cùng một lớp DP. 
7. Sau khi mọi nhiệm vụ đã được xử lý, hãy kiểm tra`dp[s1][s2]`. Nếu nó hữu hạn thì giá trị đó là thời gian tối thiểu cần thiết để hoàn thành cả hai cấp độ. Nếu nó vẫn là vô hạn thì không có nhiệm vụ nào có thể hoàn thành cả hai cấp độ, vì vậy câu trả lời là (-1). 

### Tại sao nó hoạt động 

Điều bất biến là sau khi xử lý bất kỳ tiền tố nào của danh sách nhiệm vụ được sắp xếp,`dp[j][k]`là thời gian tối thiểu trong số mỗi lần phân công có thể có của các nhiệm vụ đã xử lý có tiến trình cấp một là (j) và tiến trình cấp hai là (k), với cả hai giá trị đều được giới hạn theo yêu cầu của chúng. Mỗi nhiệm vụ có chính xác ba lựa chọn liên quan, không sử dụng, sử dụng trước khi tăng cấp hoặc sử dụng sau khi tăng cấp. Các bản ghi chuyển tiếp cấp đầu tiên tràn chính xác khi vượt qua ngưỡng, do đó trạng thái sẽ giữ lại tất cả trải nghiệm có thể quan trọng sau này. 

Đối số sắp xếp đảm bảo rằng việc xem xét các nhiệm vụ cấp một theo thứ tự tăng dần (x_i) sẽ không làm mất đi giá trị tối ưu hợp lệ. Sau khi đạt được cấp độ đầu tiên, mọi nhiệm vụ được giao cho cấp độ thứ hai đều có thể được thực hiện sau đó bất kể nó xuất hiện ở đâu trong quá trình quét DP. Do đó, mọi thực thi thực tế hợp lệ đều tương ứng với một đường dẫn DP và mọi đường dẫn DP đạt đến ((s_1,s_2)) tương ứng với một nhiệm vụ khả thi có thể được lên lịch với tất cả các nhiệm vụ cấp một trước tất cả các nhiệm vụ cấp hai. Vì mọi quá trình chuyển đổi đều lưu trữ thời gian tối thiểu cho đích đến của nó nên trạng thái cuối cùng chứa mức tối thiểu toàn cục. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

INF = 10**30

def solve():
    n, s1, s2 = map(int, input().split())

    quests = []
    for _ in range(n):
        x, t, y, r = map(int, input().split())
        quests.append((x, t, y, r))

    # An optimal solution can be represented with first-level
    # quests processed in nondecreasing x order.
    quests.sort(key=lambda q: q[0])

    # dp[j][k] = minimum time to obtain capped first-level
    # progress j and capped second-level progress k.
    dp = [[INF] * (s2 + 1) for _ in range(s1 + 1)]
    dp[0][0] = 0

    prefix_x = 0

    for x, t, y, r in quests:
        prefix_x = min(s1, prefix_x + x)

        # In-place 0/1 DP.
        # Both transitions only increase j or k, so descending
        # iteration prevents using this quest more than once.
        for j in range(prefix_x, -1, -1):
            row = dp[j]

            for k in range(s2, -1, -1):
                cur = row[k]
                if cur == INF:
                    continue

                # Use the quest after reaching level 1.
                nk = k + y
                if nk > s2:
                    nk = s2

                value = cur + r
                if value < row[nk]:
                    row[nk] = value

                # Use the quest before reaching level 1.
                if j < s1:
                    nj = j + x

                    if nj >= s1:
                        overflow = nj - s1
                        nk = k + overflow
                        if nk > s2:
                            nk = s2
                        target = dp[s1]

                        value = cur + t
                        if value < target[nk]:
                            target[nk] = value
                    else:
                        value = cur + t
                        if value < dp[nj][k]:
                            dp[nj][k] = value

    answer = dp[s1][s2]
    return str(answer if answer != INF else -1)

if __name__ == "__main__":
    print(solve())
```Bộ nhiệm vụ được lưu trữ dưới dạng`(x, t, y, r)`để quá trình chuyển đổi DP có thể sử dụng trực tiếp các giá trị được liên kết với mức hiện tại. Việc sắp xếp chỉ được thực hiện bởi`x`, bởi vì đối số thứ tự phụ thuộc vào kinh nghiệm cấp một hơn là thời gian hoặc kinh nghiệm cấp hai. 

Cái bàn có`(s1 + 1) * (s2 + 1)`tiểu bang. Cả hai thứ nguyên trải nghiệm đều bị giới hạn vì bất kỳ tiến trình nào vượt quá số lượng mà một cấp độ yêu cầu đều không có giá trị bổ sung, ngoại trừ phần vượt mức cấp một, được chuyển rõ ràng sang thứ nguyên cấp hai tại thời điểm cấp độ đầu tiên được vượt qua. 

Quá trình chuyển đổi đầu tiên bên trong vòng lặp xử lý việc sử dụng nhiệm vụ sau khi lên cấp. Quá trình chuyển đổi thứ hai xử lý việc sử dụng nó trước khi lên cấp. Thứ tự của hai quá trình chuyển đổi này không ảnh hưởng đến tính chính xác vì cả hai đích đều nằm xa hơn theo hướng DP giảm dần. 

Việc lặp đi lặp lại giảm dần là tinh tế. Nếu như`j < s1`, sử dụng nhiệm vụ ở cấp độ đầu tiên sẽ tăng lên`j`, vì vậy hàng đích của nó đã được xử lý. Sử dụng nó ở cấp độ thứ hai tăng lên`k`, do đó cột đích của nó cũng đã được xử lý. Điều này có nghĩa là không quá trình chuyển đổi nào có thể đưa nhiệm vụ hiện tại trở lại trạng thái sẽ được xử lý lại. 

Việc tính toán tràn sử dụng`j + x - s1`, không`min(j + x, s1) - s1`. Cái trước giữ lại chính xác số tiền vượt qua ranh giới cấp một. Giá trị cấp hai được giới hạn ở`s2`vì kinh nghiệm bổ sung sau khi hoàn thành cấp độ thứ hai không có tác dụng. 

Số nguyên Python không bị tràn, nhưng các ngôn ngữ sử dụng số nguyên có chiều rộng cố định cần loại 64 bit. Tổng thời gian tối đa có thể là nhiều nhất (500 \cdot 10^9 = 5 \cdot 10^{11}). 

Giải pháp cuộc thi ban đầu sử dụng cùng một DP được sắp xếp theo (x_i) với thời gian (O(n s_1s_2)) và bộ nhớ (O(s_1s_2)). 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
2 100 100
100 100 10 10
101 11 100 10
```Các nhiệm vụ đã được sắp xếp theo kinh nghiệm cấp độ đầu tiên. Các trạng thái hữu ích nhất được hiển thị dưới đây. 

| Sau nhiệm vụ | Tình trạng`(j, k)`| Thời gian | Ý nghĩa | 
| --- | --- | --- | --- | 
| Bắt đầu |`(0, 0)`| 0 | Không có nhiệm vụ nào được sử dụng | 
| 1 |`(100, 0)`| 100 | Nhiệm vụ đầu tiên hoàn thành chính xác cấp 1 | 
| 1 |`(0, 10)`| 10 | Nhiệm vụ đầu tiên được dành riêng cho cấp 2 | 
| 2 |`(100, 100)`| 110 | Nhiệm vụ đầu tiên được sử dụng trước khi lên cấp, nhiệm vụ thứ hai sau đó | 
| 2 |`(100, 10)`| 21 | Nhiệm vụ thứ hai được sử dụng trước khi lên cấp sau khi đặt trước | 
| 2 |`(0, 100)`| 20 | Cả hai nhiệm vụ dành riêng cho cấp 2 | 

Trạng thái tối ưu là`(100,100)`với chi phí 110. Steve hoàn thành nhiệm vụ đầu tiên ở cấp 1, đạt đúng 100 kinh nghiệm, sau đó hoàn thành nhiệm vụ thứ hai ở cấp 2 để nhận thêm 100 kinh nghiệm. 

Phương án thay thế với giá 21 không hoàn thành cấp độ thứ hai vì nó chỉ có 10 kinh nghiệm cấp độ hai. DP giữ chính xác cả hai chiều để phân biệt các trạng thái này. 

### Mẫu 2 

Đầu vào là```
4 20 20
40 1000 20 20
6 6 5 5
10 10 1 1
10 10 1 1
```Sau khi sắp xếp theo (x_i), nhiệm vụ có kinh nghiệm cấp 1 là 6, 10, 10 và 40. 

Lộ trình tối ưu bỏ qua nhiệm vụ với (x=6), sử dụng cả nhiệm vụ 10 kinh nghiệm trước cấp độ đầu tiên và sử dụng nhiệm vụ 40 kinh nghiệm sau khi lên cấp. 

| Sau nhiệm vụ | Tình trạng`(j, k)`| Thời gian | Lý do | 
| --- | --- | --- | --- | 
| Bắt đầu |`(0, 0)`| 0 | Không có nhiệm vụ | 
| (x=6) |`(6, 0)`| 6 | Đạt nó ở cấp độ 1 | 
| (x=10) |`(16, 0)`| 16 | Nhiệm vụ 10 kinh nghiệm đầu tiên ở cấp 1 | 
| (x=10) |`(20, 0)`| 20 | Hai nhiệm vụ 10 kinh nghiệm hoàn thành cấp 1 | 
| (x=40) |`(20, 20)`| 40 | Nhiệm vụ 40 kinh nghiệm được sử dụng ở cấp 2 | 

Nhiệm vụ với (x=40) tốn 1000 phút ở cấp độ đầu tiên, vì vậy sử dụng nó sẽ rất tai hại. Phiên bản cấp hai của nó chỉ tốn 20 phút và cung cấp chính xác 20 trải nghiệm cần thiết. DP tìm thấy nhiệm vụ này vì mỗi nhiệm vụ độc lập có cả hai lựa chọn cấp độ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n s_1 s_2)) | Mỗi nhiệm vụ kiểm tra tối đa ((s_1+1)(s_2+1)) trạng thái | 
| Không gian | (O(s_1 s_2)) | Chỉ có bảng DP hai chiều hiện tại được lưu trữ | 

Với (n,s_1,s_2\le500), số lần lặp trạng thái trong trường hợp xấu nhất là (500\cdot501\cdot501=125{,}500{,}500), cộng với số lần chuyển đổi không đổi từ mỗi trạng thái có thể truy cập. Yêu cầu bộ nhớ chỉ khoảng 251.000 mục DP, tức là dưới 256 MB. Thuật toán phù hợp với độ phức tạp mà ban biên tập cuộc thi đưa ra. 

## Trường hợp thử nghiệm```python
# The solution above exposes solve(), which reads from the global
# input function and returns the answer as a string.

import sys
import io

def run(inp: str) -> str:
    global input

    old_stdin = sys.stdin
    old_input = input

    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    try:
        return solve().strip()
    finally:
        sys.stdin = old_stdin
        input = old_input

# Provided samples
assert run(
    """2 100 100
100 100 10 10
101 11 100 10
"""
) == "110", "sample 1"

assert run(
    """4 20 20
40 1000 20 20
6 6 5 5
10 10 1 1
10 10 1 1
"""
) == "40", "sample 2"

assert run(
    """2 20 5
10 10 5 5
10 10 5 5
"""
) == "-1", "sample 3"

# Minimum-size input.
assert run(
    """1 1 1
1 5 1 2
"""
) == "-1", "one quest cannot finish both levels"

# Exact first-level boundary, followed by the second level.
assert run(
    """2 5 5
5 7 1 1
1 100 5 2
"""
) == "9", "exact level-up boundary"

# Overflow must be transferred to the second level.
assert run(
    """2 5 5
6 10 1 1
1 1 5 1
"""
) == "11", "first-level overflow"

# Maximum n, while keeping the experience dimensions tiny enough
# for a fast regression test.
quests = "\n".join(
    "2 3 1 1"
    for _ in range(500)
)

assert run(
    "500 1 1\n" + quests + "\n"
) == "4", "maximum n"

# All quests have identical statistics.
assert run(
    """4 2 2
1 5 1 1
1 5 1 1
1 5 1 1
1 5 1 1
"""
) == "12", "identical quests"

# Crossing the first-level boundary by more than one experience point.
assert run(
    """2 5 5
8 10 1 1
1 1 5 1
"""
) == "11", "large overflow"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1`với một nhiệm vụ |`-1`| Không thể có kích thước tối thiểu | 
|`2 5 5`với nhiệm vụ 5 kinh nghiệm chính xác |`9`| Ranh giới chính xác không tràn | 
|`2 5 5`với nhiệm vụ 6 kinh nghiệm đầu tiên |`11`| Chuyển tràn | 
|`500 1 1`với các nhiệm vụ giống hệt nhau |`4`| Tối đa (n) | 
| Bốn nhiệm vụ giống hệt nhau với (s_1=s_2=2) |`12`| Số liệu thống kê bằng nhau lặp đi lặp lại và ngăn chặn việc tái sử dụng nhiệm vụ | 
| Nhiệm vụ đầu tiên mang lại 8 khi chỉ cần 5 |`11`| Tràn lớn hơn một điểm kinh nghiệm | 

## Vỏ cạnh 

Trường hợp quan trọng đầu tiên là hoàn thành chính xác cấp độ đầu tiên. Coi như```
2 5 5
5 7 1 1
1 100 5 2
```Nhiệm vụ đầu tiên mang lại chính xác 5 điểm kinh nghiệm, vì vậy mức tràn của nó bằng 0. Steve dành 7 phút cho nó, đạt đến cấp độ thứ hai, sau đó sử dụng nhiệm vụ thứ hai để nhận 5 điểm kinh nghiệm với chi phí là 2. DP di chuyển từ`(0,0)`ĐẾN`(5,0)`với giá 7 thì đến`(5,5)`với chi phí là 9. Câu trả lời là`9`. các`>= s1`ranh giới trong quá trình chuyển đổi xử lý trường hợp này một cách chính xác vì tràn số 0 là hợp lệ. 

Trường hợp cạnh thứ hai là tràn chính hãng. Coi như```
2 5 5
6 10 1 1
1 1 5 1
```Sử dụng nhiệm vụ đầu tiên ở cấp độ 1 sẽ tạo ra 6 kinh nghiệm, vì vậy cấp độ đầu tiên tiêu tốn 5 và 1 kinh nghiệm sẽ được chuyển sang cấp độ thứ hai. Nhiệm vụ thứ hai sau đó cung cấp thêm 5 kinh nghiệm, mang lại tổng cộng 6 tiến bộ ở cấp độ thứ hai. Sự chuyển đổi DP từ`(0,0)`ĐẾN`(5,1)`ghi lại tràn và quá trình chuyển đổi tiếp theo đạt đến`(5,5)`. Tổng thời gian là`10+1=11`. 

Trường hợp cạnh thứ ba là không thể:```
2 20 5
10 10 5 5
10 10 5 5
```Hai nhiệm vụ có thể cung cấp chính xác 20 kinh nghiệm trước cấp độ đầu tiên, nhưng sau đó cả hai nhiệm vụ đều đã được sử dụng hết. Nhà nước`(20,0)`có thể truy cập được, trong khi`(20,5)`thì không. Từ`dp[20][5]`vẫn là vô cùng, thuật toán trả về`-1`. 

Trường hợp thứ tư là một nhiệm vụ cực kỳ tốn kém trước khi lên cấp nhưng sau đó lại rẻ. Trong Mẫu 2, nhiệm vụ 40 kinh nghiệm tốn 1000 phút trước khi lên cấp và chỉ 20 phút sau đó. DP không sử dụng quy tắc tham lam chỉ dựa trên kinh nghiệm. Nó xem xét cả hai vai trò và phát hiện ra rằng hai nhiệm vụ 10 kinh nghiệm sẽ hoàn thành cấp độ đầu tiên trong 20 phút, sau đó nhiệm vụ 40 kinh nghiệm sẽ hoàn thành cấp độ thứ hai trong 20 phút nữa. Câu trả lời kết quả là`40`. 

Trường hợp cạnh thứ năm là có nhiều nhiệm vụ có giá trị giống hệt nhau. Với```
4 2 2
1 5 1 1
1 5 1 1
1 5 1 1
1 5 1 1
```hai nhiệm vụ phải được sử dụng trước khi lên cấp và hai nhiệm vụ sau đó. Tổng chi phí là (2\cdot5+2\cdot1=12). Vòng lặp giảm dần tại chỗ an toàn ngay cả với các nhiệm vụ giống hệt nhau vì mỗi nhiệm vụ được xử lý riêng biệt và không thể chuyển sang trạng thái sẽ được truy cập lại trong cùng lần lặp đó. 

Trường hợp tinh vi cuối cùng là tràn lớn. Với```
2 5 5
8 10 1 1
1 1 5 1
```nhiệm vụ đầu tiên đóng góp 3 kinh nghiệm tràn sau khi hoàn thành cấp độ đầu tiên. Nhiệm vụ thứ hai sau đó cung cấp thêm 5 điểm kinh nghiệm, vì vậy cấp độ thứ hai nhận được tổng cộng 8 điểm kinh nghiệm. Câu trả lời là một lần nữa`11`. Điều này xác nhận rằng phần tràn phải được thêm vào trước khi giới hạn tiến trình cấp hai, không bị loại bỏ khi trạng thái cấp một đạt đến (s_1).
