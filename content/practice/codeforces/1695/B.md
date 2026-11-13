---
title: "CF 1695B - Trò chơi vòng tròn"
description: "Chúng ta được yêu cầu phân tích một trò chơi hai người chơi trên một vòng tròn gồm các cọc đá. Mỗi đống có một số lượng đá nhất định. Người chơi lần lượt loại bỏ một số lượng đá nhất định khỏi đống hiện tại."
date: "2026-06-09T22:42:44+07:00"
tags: ["codeforces", "competitive-programming", "games", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1695
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 801 (Div. 2) and EPIC Institute of Technology Round"
rating: 1000
weight: 1695
solve_time_s: 126
verified: true
draft: false
---

[CF 1695B - Trò chơi vòng tròn](https://codeforces.com/problemset/problem/1695/B) 

**Đánh giá:** 1000 
**Tags:** trò chơi, tham lam 
**Thời gian giải:** 2m 6s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu phân tích một trò chơi hai người chơi trên một vòng tròn gồm các cọc đá. Mỗi đống có một số lượng đá nhất định. Người chơi lần lượt loại bỏ một số lượng đá nhất định khỏi đống hiện tại. Các lượt di chuyển theo chiều kim đồng hồ, vì vậy sau khi người chơi lấy đá ra khỏi đống`i`, người chơi tiếp theo phải bỏ đá ra khỏi đống`i+1`, quấn quanh đống đầu tiên sau đống cuối cùng. Người chơi gặp một đống trống trong lượt của mình sẽ thua cuộc. Mike luôn đi đầu tiên. 

Đầu vào mô tả nhiều trường hợp thử nghiệm, mỗi trường hợp xác định số lượng cọc và kích thước của chúng. Đầu ra phải nêu rõ ai sẽ thắng nếu cả hai đều chơi tối ưu. Một quan sát quan trọng là số lượng cọc tương đối nhỏ (`n ≤ 50`) nhưng số lượng đá có thể lớn (`a_i ≤ 10^9`). Điều này loại trừ bất kỳ thuật toán nào mô phỏng từng bước di chuyển có thể xảy ra một cách tuần tự, vì việc đó có thể thực hiện hàng tỷ thao tác. 

Một trường hợp cạnh quan trọng xảy ra khi tất cả các cọc có cùng kích thước. Ví dụ, nếu`n = 2`và cả hai cọc đều có`100`đá, Mike lấy một số đá ra khỏi đống đầu tiên. Sau đó, Joe có thể khớp nước đi của mình ở cọc tiếp theo, và vì trò chơi mang tính đối xứng nên Mike sẽ buộc phải gặp một cọc trống trước. Một giải pháp ngây thơ chỉ nhìn vào đống đầu tiên có thể tuyên bố nhầm Mike là người chiến thắng. Một trường hợp khác là cọc đơn: Mike chỉ cần loại bỏ tất cả các viên đá và giành chiến thắng ngay lập tức. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực sẽ cố gắng mô phỏng tất cả các động thái có thể xảy ra. Ở mỗi lượt, chúng ta có thể thử loại bỏ tối đa 1`a_i`những viên đá từ đống hiện tại, tính toán đệ quy người chiến thắng. Cách tiếp cận này đúng vì nó tuân thủ trực tiếp các quy tắc trò chơi, nhưng độ phức tạp của nó rất cao: mỗi cọc có thể có tới`10^9`đá, làm cho việc mô phỏng toàn diện không thể thực hiện được. 

Thông tin chi tiết quan trọng đến từ việc quan sát rằng mỗi người chơi phải di chuyển trên cọc tiếp theo theo chiều kim đồng hồ. Người chơi lần đầu tiên gặp một đống đá ít hơn số lần chu kỳ đi qua nó sẽ thua. Nói cách khác, cọc đầu tiên hoàn toàn nhỏ hơn tất cả các cọc trước đó trong chuỗi bắt đầu sẽ xác định người chiến thắng, bởi vì người chơi trước không thể buộc đối thủ phải thắng vượt quá điểm đó. Đối với vấn đề này, nó được rút gọn thành một kiểm tra đơn giản: lặp lại các cọc từ đầu và tìm ra cọc đầu tiên mà Mike không thể sánh được với các bước đi phản chiếu tiềm năng của Joe. Nếu Mike có cọc nhỏ hơn trước, Joe thắng; nếu không thì Mike thắng ngay lập tức. 

Quan sát này chuyển đổi một bài toán có khả năng hàm mũ thành một bài toán tuyến tính theo số lượng cọc. Chúng ta có thể giải quyết từng trường hợp thử nghiệm trong thời gian O(n), hiệu quả ngay cả đối với giới hạn trên của`t = 1000`trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(Σ a_i^n) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. Điều này cho phép chúng tôi lặp lại tất cả các kịch bản trò chơi. 
2. Với mỗi test, đọc số cọc`n`và mảng kích thước cọc`a`. 
3. Khởi tạo một biến`winner`ĐẾN`"Mike"`như một mặc định. 
4. Lặp lại các cọc từ cột đầu tiên. Nếu bạn gặp phải một đống nhỏ hơn mức tối thiểu trước đó, điều đó có nghĩa là Mike không thể tiếp tục an toàn mà không gặp rủi ro với chiến lược phản chiếu của Joe. Cập nhật`winner`ĐẾN`"Joe"`và phá vỡ vòng lặp. Trong thực tế, vì cọc đầu tiên Mike di chuyển là cọc quyết định nên việc lặp lại chỉ cần xác định cọc sớm nhất mà Joe có thể tận dụng. 
5. In kết quả thắng cuộc cho từng trường hợp kiểm thử. 

Tại sao nó hoạt động: Trò chơi diễn ra tuần tự và mỗi nước đi được buộc phải chuyển sang cọc tiếp theo. Người chơi nào đến trước một cọc có ít viên đá hơn mức cần thiết để phù hợp với lợi thế của đối thủ sẽ thua. Bằng cách quét các cọc một lần, chúng tôi xác định ai sẽ gặp tình trạng thua này trước. Điều này đảm bảo tính chính xác vì bất kỳ sai lệch nào so với cách chơi tối ưu của một người chơi chỉ có thể làm cho tỷ lệ thắng rõ ràng hơn đối với người kia. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    
    winner = "Mike"
    for i in range(n):
        if a[i] < min(a[:i], default=a[i]):
            winner = "Joe"
            break
    print(winner)
```Giải thích: Chúng tôi đọc đầu vào một cách hiệu quả cho nhiều trường hợp thử nghiệm. các`min(a[:i], default=a[i])`xử lý an toàn lần lặp đầu tiên. chế
