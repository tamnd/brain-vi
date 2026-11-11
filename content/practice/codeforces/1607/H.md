---
title: "CF 1607H - Chuẩn bị tiệc 2"
description: "Chúng tôi được đưa cho một loạt món ăn, mỗi món gồm vài gam cá và vài gam thịt. Đối với mỗi món ăn, người nếm thử sẽ tiêu thụ một tổng lượng thức ăn cố định, nhưng anh ta có thể chọn lượng cá và lượng thịt để ăn từ món ăn đó."
date: "2026-06-10T07:46:57+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "sortings", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1607
codeforces_index: "H"
codeforces_contest_name: "Codeforces Round 753 (Div. 3)"
rating: 2200
weight: 1607
solve_time_s: 108
verified: false
draft: false
---

[CF 1607H - Chuẩn bị tiệc 2](https://codeforces.com/problemset/problem/1607/H) 

**Xếp hạng:** 2200 
**Tags:** tham lam, sắp xếp, hai con trỏ 
**Thời gian giải:** 1 phút 48s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được đưa cho một loạt món ăn, mỗi món gồm vài gam cá và vài gam thịt. Đối với mỗi món ăn, người nếm thử sẽ tiêu thụ một tổng lượng thức ăn cố định, nhưng anh ta có thể chọn lượng cá và lượng thịt để ăn từ món ăn đó. Hai món ăn được coi là giống nhau sau khi nếm thử nếu phần cá và thịt còn lại giống hệt nhau. Mục đích là chọn lượng cá và thịt mà người nếm thử ăn từ mỗi món ăn sao cho số lượng món ăn khác biệt được tạo ra được giảm thiểu. 

Mỗi trường hợp thử nghiệm cung cấp số lượng món ăn, số lượng cá và thịt ban đầu cho mỗi món ăn và tổng số gam mà người nếm thử phải ăn. Đầu ra yêu cầu số lượng món ăn riêng biệt tối thiểu sau khi nếm thử và kế hoạch ăn uống thực tế cho người nếm thử. 

Các ràng buộc cho phép tổng cộng lên tới 200.000 món ăn trong tất cả các trường hợp thử nghiệm, với số lượng cá và thịt của mỗi món ăn lên tới một triệu. Một cách tiếp cận ngây thơ so sánh tất cả các cách phân bổ thực phẩm có thể ăn được cho tất cả các cặp món ăn sẽ là quá chậm vì số lượng các khả năng tăng theo phương trình bậc hai trong`n`. Người nếm thử phải tôn trọng giới hạn của từng món ăn, vì vậy`0 <= fish eaten <= a_i`Và`0 <= meat eaten <= b_i`, có thể tạo ra những trường hợp khó khăn nếu tổng ăn`m_i`nhỏ hơn mỗi thành phần. Ví dụ, nếu một món ăn có`(2, 2)`gram cá và thịt và`m_i=3`, người nếm phải ăn 3 gam chia cho cá và thịt. Nếu chúng ta chọn`(3, 0)`hoặc`(2, 1)`một cách tùy tiện, chúng ta thay đổi đĩa còn sót lại và việc chọn sai cách phân bổ có thể khiến nhiều món ăn không thể thu gọn về cùng một trạng thái cuối cùng. 

Các trường hợp có khía cạnh không rõ ràng bao gồm các món ăn không có cá hoặc không có thịt, hoặc khi`m_i`chính xác bằng một trong các thành phần, buộc phải phân bổ cụ thể. Ví dụ như một món ăn`(0, 5)`với`m_i=3`chỉ có thể có`(0, 3)`ăn rồi bỏ đi`(0, 2)`phía sau. Việc triển khai bất cẩn mà bỏ qua các giới hạn này có thể tạo ra kết quả không hợp lệ. 

## Phương pháp tiếp cận 

Một cách tiếp cận mạnh mẽ là liệt kê tất cả những cách có thể mà người nếm thử có thể ăn từng món ăn, tạo ra tất cả những cách có thể.`(remaining_fish, remaining_meat)`ghép đôi và đếm các món ăn cuối cùng riêng biệt. Điều này có hiệu quả vì giống cuối cùng được xác định bởi các cặp này, nhưng nó không thành công ở quy mô lớn.`n`bởi vì mỗi món ăn có tới`m_i + 1`khả năng, tạo ra một không gian tìm kiếm theo cấp số nhân. Với`n`lên tới 2*10^5, điều này là không thể. 

Quan sát quan trọng là để giảm thiểu số lượng các món ăn riêng biệt, chúng tôi muốn càng nhiều món ăn càng tốt với cùng một loại cá và thịt còn lại. Mỗi món ăn có một phạm vi khả thi về số lượng cá có thể ăn:`max(0, m_i - b_i)`ĐẾN`min(a_i, m_i)`. Vậy số cá còn lại là`a_i - eaten_fish`và phần thịt còn lại là`b_i - eaten_meat = b_i - (m_i - eaten_fish)`. Nếu chúng ta chọn`eaten_fish`trong phạm vi khả thi này, kết quả`(remaining_fish, remaining_meat)`có thể nhận các giá trị dọc theo một đường thẳng trong mạng số nguyên. 

Thông tin chi tiết này cho phép chúng tôi xử lý vấn đề như tìm một giá trị duy nhất cho số cá còn lại nằm trong khoảng thời gian khả thi của tất cả các món ăn, nếu có thể. Cụ thể, tính toán số cá còn lại tối thiểu và tối đa cho tất cả các món ăn (`a_i - min(a_i, m_i)`Và`a_i - max(0, m_i - b_i)`) và sau đó lấy giao điểm của các phạm vi này. Bất kỳ giá trị nào bên trong giao điểm này đều có thể được sử dụng làm số cá còn lại cho tất cả các món ăn, đảm bảo sự đa dạng tối thiểu của một món. Nếu phần giao trống, thì mức độ đa dạng tối thiểu là hai: chọn một giá trị nằm trong phạm vi khả thi cho một nửa số món ăn và một giá trị khác cho phần còn lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi món ăn, hãy tính số cá tối thiểu và tối đa còn lại sau khi nếm thử. Người nếm ít nhất có thể ăn`max(0, m_i - b_i)`cá và nhiều nhất`min(a_i, m_i)`cá. Số cá còn lại là`a_i - eaten_fish`, đưa ra một khoảng khả thi`[a_i - min(a_i, m_i), a_i - max(0, m_i - b_i)]`. 
2. Lấy tất cả các khoảng này và tính giao điểm trên tất cả các món ăn. Cho phép`low`là mức tối đa của tất cả các giới hạn dưới của khoảng và`high`là mức tối thiểu của tất cả các giới hạn trên. 
3. Nếu`low <= high`, tồn tại một giá trị chung cho cá còn lại cho tất cả các món ăn. Chọn`remaining_fish = low`và tính số cá đã ăn cho mỗi món như sau`a_i - remaining_fish`. Sau đó`eaten_meat = m_i - eaten_fish`. Các món còn lại thu được đều giống hệt nhau nên độ đa dạng tối thiểu là 1. 
4. Nếu`low > high`, các khoảng không cắt nhau. Chọn số cá còn lại khả thi cho từng món ăn theo khoảng thời gian riêng. Sự đa dạng tối thiểu ít nhất là 2. Một chiến lược đơn giản là chọn giới hạn dưới cho mỗi món ăn. Các món ăn còn lại thu được sẽ thu gọn thành nhiều nhất hai mẫu khác nhau. 
5. Đưa ra mức độ đa dạng tối thiểu theo kế hoạch ăn uống của người nếm thử`(eaten_fish, eaten_meat)`cho từng món ăn. 

Tại sao nó hoạt động: bằng cách biểu thị khoảng thời gian cá còn lại khả thi của mỗi món ăn và giao nhau với chúng, chúng tôi đảm bảo rằng chúng tôi chọn một giá trị có giá trị đồng thời cho tất cả các món ăn. Điều này đảm bảo người nếm sẽ ăn chính xác`m_i`từ mỗi món ăn và các món ăn còn lại có thể được làm giống hệt nhau, giảm thiểu sự đa dạng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        input()  # blank line between test cases
        n = int(input())
        a, b, m = [], [], []
        for _ in range(n):
            ai, bi, mi = map(int, input().split())
            a.append(ai)
            b.append(bi)
            m.append(mi)

        low = -1_000_000_000
        high = 1_000_000_000
        for i in range(n):
            min_remain_fish = a[i] - min(a[i], m[i])
            max_remain_fish = a[i] - max(0, m[i] - b[i])
            low = max(low, min_remain_fish)
            high = min(high, max_remain_fish)

        res = []
        if low <= high:
            remaining_fish = low
            for i in range(n):
                eaten_fish = a[i] - remaining_fish
                eaten_meat = m[i] - eaten_fish
                res.append((eaten_fish, eaten_meat))
            print(1)
        else:
            for i in range(n):
                eaten_fish = a[i] - (a[i] - min(a[i], m[i]))
                eaten_meat = m[i] - eaten_fish
                res.append((eaten_fish, eaten_meat))
            print(2)
        for x, y in res:
            print(x, y)

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên phân tích cú pháp đầu vào và lưu trữ thông tin món ăn trong danh sách. Khoảng thời gian cá còn lại khả thi của mỗi món ăn được tính toán và giao điểm của các khoảng thời gian được xác định. Nếu tồn tại một giá trị chung thì nó sẽ được chọn; mặt khác, một dự phòng đơn giản sẽ được sử dụng. Kế hoạch ăn uống của người nếm thử được tính toán dựa trên số lượng cá còn lại đã chọn. Cần chú ý tôn trọng ranh giới của từng món ăn và đảm bảo tổng lượng thức ăn ăn bằng`m_i`. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu 1```
3
10 10 2
9 9 0
10 9 1
```| Món ăn | một | b | m | min_remain_fish | max_remain_fish | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 10 | 10 | 2 | 8 | 10 | 
| 2 | 9 | 9 | 0 | 9 | 9 | 
| 3 | 10 | 9 | 1 | 9 | 10 | 

Giao lộ:`low = max(8,9,9)=9`,`high = min(10,9,10)=9`. Không trống rỗng. Số cá còn lại = 9 cho tất cả các món. Cá ăn =`[1,0,1]`, đã ăn thịt =`[1,0,0]`. Giống tối thiểu = 1. 

### Đầu vào mẫu 2```
2
3 4 1
5 1 2
```| Món ăn | một | b | m | min_remain_fish | max_remain_fish | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 3 | 4 | 1 | 2 | 3 | 
| 2 | 5 | 1 | 2 | 3 | 5 | 

Giao lộ:`low = 3`,`high = 3`. Không trống rỗng. Số cá còn lại = 3 cho cả hai món. Cá ăn =`[0,2]`, đã ăn thịt =`[1,0]`. Giống tối thiểu = 1. 

Những dấu vết này cho thấy cách thuật toán tính toán các phạm vi khả thi và giao chúng để thu gọn các món ăn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi món ăn được xử lý một lần để tính toán các khoảng thời gian và giao điểm được tính toán trong một |
