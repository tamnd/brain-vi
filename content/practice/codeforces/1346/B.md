---
title: "CF 1346B - Chương trình đào tạo"
description: "Vấn đề yêu cầu chúng ta sắp xếp số lượng bài giảng tối đa trong chương trình đào tạo lập trình kéo dài n ngày. Mỗi ngày hoặc là một ngày bình thường, nơi chúng ta có thể tổ chức các bài giảng, hoặc là một ngày du ngoạn, nơi không được phép giảng bài."
date: "2026-06-11T14:51:45+07:00"
tags: ["codeforces", "competitive-programming", "*special", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1346
codeforces_index: "B"
codeforces_contest_name: "Kotlin Heroes: Episode 4"
rating: 1400
weight: 1346
solve_time_s: 190
verified: false
draft: false
---

[CF 1346B - Chương trình đào tạo](https://codeforces.com/problemset/problem/1346/B) 

**Đánh giá:** 1400 
**Tags:** *đặc biệt, tham lam 
**Thời gian giải:** 3 phút 10s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Vấn đề yêu cầu chúng tôi sắp xếp số lượng bài giảng tối đa trong chương trình đào tạo lập trình kéo dài`n`ngày. Mỗi ngày hoặc là một ngày bình thường, nơi chúng ta có thể tổ chức các bài giảng, hoặc là một ngày du ngoạn, nơi không được phép giảng bài. Vào những ngày bình thường, chúng tôi không thể lên lịch nhiều hơn`k_1`bài giảng trong bất kỳ ngày nào và trong bất kỳ cặp ngày liên tiếp nào, tổng số bài giảng không được vượt quá`k_2`. Mục tiêu là xác định tổng số bài giảng lớn nhất có thể được giao trong tất cả các ngày, đồng thời tôn trọng những hạn chế này. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm cung cấp số ngày, giới hạn bài giảng mỗi ngày và mỗi cặp và một chuỗi nhị phân biểu thị ngày nào là chuyến du ngoạn. Đầu ra là một số nguyên cho mỗi trường hợp thử nghiệm, tổng số bài giảng tối đa có thể. 

Những hạn chế chính là`n`có thể lớn tới 5000,`k_1`Và`k_2`có thể lên tới 200.000 và có thể có tới 50 trường hợp thử nghiệm. Điều này cho thấy rằng một thuật toán có độ phức tạp`O(n^2)`mỗi trường hợp thử nghiệm sẽ quá chậm trong trường hợp xấu nhất, bởi vì`5000^2 * 50`hoạt động ở mức hàng tỷ. Chúng ta cần một cách tiếp cận hoạt động theo thời gian tuyến tính hoặc tuyến tính cho mỗi trường hợp thử nghiệm. 

Một khía cạnh tinh tế là những ngày liên tiếp ảnh hưởng lẫn nhau thông qua`k_2`hạn chế. Một cách tiếp cận ngây thơ có thể cố gắng gán`k_1`giảng bài vào mỗi ngày không có chuyến tham quan, nhưng điều này có thể vi phạm ràng buộc về tổng theo cặp. Một trường hợp biên khác xảy ra khi nhiều ngày tham quan liên tiếp hoặc ở các biên; thuật toán phải thiết lập lại chính xác việc tích lũy bài giảng trong các phân đoạn đó. 

Ví dụ, nếu chúng ta có`n = 4`,`k_1 = 3`,`k_2 = 4`và chuỗi ngày là`1101`, sau đó một cách tiếp cận ngây thơ gán`k_1`mọi nơi sẽ mang lại`[3, 3, 0, 3]`, Nhưng`3 + 3 = 6`vượt quá`k_2 = 4`. Phép gán đúng là`[2, 2, 0, 3]`, thu được tổng số 7. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là lặp đi lặp lại mỗi ngày không có chuyến tham quan và thử tất cả số bài giảng có thể có từ`0`ĐẾN`k_1`, trong khi kiểm tra ràng buộc tổng số ngày liên tiếp. Trong mỗi ngày, chúng tôi sẽ theo dõi tổng số tích lũy có thể có và số liệu hợp lệ của ngày cuối cùng. Điều này có thể được thực hiện như một giải pháp lập trình động với`dp[i][x]`đại diện cho tổng số bài giảng tối đa cho đến ngày`i`nếu ngày`i`có`x`bài giảng. Mỗi bản cập nhật sẽ yêu cầu kiểm tra tất cả`x`Và`y`cặp trong những ngày liên tiếp. Trong trường hợp xấu nhất, với`n = 5000`Và`k_1 = 200,000`, phương pháp này không khả thi vì nó đòi hỏi`O(n * k_1^2)`hoạt động. 

Cái nhìn sâu sắc quan trọng đến từ việc quan sát rằng`k_2`ràng buộc có thể được biểu diễn dưới dạng mức trần về số tiền mà một ngày có thể tiêu tốn so với người hàng xóm của nó. Nếu như`k_2 >= 2 * k_1`, thì ràng buộc liên tiếp không bao giờ bị ràng buộc và chúng ta có thể chỉ cần gán`k_1`đến mọi ngày không tham quan. Bằng không, nếu`k_2 < 2 * k_1`, thì đối với mỗi cặp ngày không tham quan liên tiếp, số tiền tối đa chúng tôi có thể chỉ định cho mỗi cặp bị giới hạn bởi`floor(k_2 / 2)`. Điều này làm giảm vấn đề chia chương trình đào tạo thành các khối liền kề của những ngày không tham quan và chỉ định một trong hai`k_1`hoặc`floor(k_2 / 2)`mỗi ngày, luân phiên khi cần thiết, tương tự như bài toán “sơn hàng rào” hàng xóm ép buộc nhau. 

Quan sát này cho phép một giải pháp thời gian tuyến tính. Chúng ta có thể quét chuỗi ngày, xác định các phân đoạn của những ngày không tham quan liên tiếp và đối với mỗi phân đoạn, tính toán số bài giảng tối đa bằng cách điền xen kẽ các giá trị tối đa cho phép mà không bao giờ vượt quá.`k_2`trên bất kỳ cặp liên tiếp nào. Những ngày tham quan đóng vai trò như những dải phân cách tự nhiên giúp thiết lập lại sự luân phiên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * k_1^2) | O(n * k_1) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. 
2. Với mỗi test case, phân tích cú pháp`n`,`k_1`,`k_2`và chuỗi ngày. 
3. Tính toán`max_pair = min(k_1, k_2 // 2)`. Điều này thể hiện số lượng bài giảng tối đa có thể được phân công một cách an toàn cho một ngày khi xem xét giới hạn ngày liên tiếp. 
4. Khởi tạo bộ đếm tổng số bài giảng. 
5. Quét qua chuỗi ngày từ trái sang phải. Duy trì độ dài của những ngày không tham quan liên tiếp. 
6. Bất cứ khi nào gặp một ngày du ngoạn hoặc phần cuối của chuỗi, hãy xử lý phân đoạn trước đó của những ngày không tham quan liên tiếp. Đối với một đoạn có độ dài`L`, tổng số bài giảng tối đa là`L * max_pair + (L // 2) * (k_2 - 2 * max_pair)`nếu như`k_2 < 2 * k_1`, hoặc đơn giản là`L * k_1`nếu như`k_2 >= 2 * k_1`. 
7. Đặt lại bộ đếm phân đoạn vào mỗi ngày tham quan và tiếp tục. 
8. Sau khi xử lý tất cả các phân đoạn, xuất ra tổng số bài giảng tích lũy. 

Điều bất biến là ở mọi phân đoạn của những ngày không tham quan liên tiếp, các bài giảng được giao không bao giờ vi phạm giới hạn mỗi ngày`k_1`và giới hạn tổng số ngày liên tiếp`k_2`. Ngày tham quan đương nhiên sẽ phân chia các phân đoạn nên chúng ta không cần xem xét vi phạm xuyên biên giới. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, k1, k2 = map(int, input().split())
    s = input().strip()
    
    max_pair = min(k1, k2 // 2)
    total = 0
    count = 0
    
    for ch in s + '0':
        if ch == '1':
            count += 1
        else:
            if count > 0:
                # maximum lectures in this segment
                segment_total = count * max_pair + (count // 2) * (k2 - 2 * max_pair)
                total += segment_total
                count = 0
    print(total)
```Giải pháp quét mỗi ngày đúng một lần và xử lý các phân đoạn của những ngày không tham quan liên tiếp. Đang bổ sung`'0'`ở cuối đảm bảo phân đoạn cuối cùng được xử lý mà không cần kiểm tra ranh giới đặc biệt. Việc sử dụng phép chia số nguyên đảm bảo chúng tôi tôn trọng giới hạn ngày liên tiếp khi chia các bài giảng bổ sung. 

## Ví dụ đã hoạt động 

**Ví dụ 1** 

đầu vào:`4 5 7`, ngày:`1011`. 

Các phân đoạn của ngày không tham quan là`[1]`,`[1, 1]`.`k2 // 2 = 3`,`max_pair = min(5, 3) = 3`. 

| Phân đoạn | Chiều dài | Bài giảng được tính toán | tổng số
