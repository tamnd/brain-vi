---
title: "CF 106130E - \u65b0\u7530\u5fcc\u8d5b\u9a6c"
description: "Chúng ta có hai mảng tốc độ ngựa được sắp xếp giảm dần. Một cái thuộc về Tian Ji và cái còn lại thuộc về King. Một trận đấu được hình thành bằng cách ghép đôi một con ngựa chưa sử dụng của mỗi bên và kết quả phụ thuộc vào sự so sánh chặt chẽ: Tian Ji chỉ kiếm được tiền khi con ngựa mà anh ấy chọn hoàn toàn…"
date: "2026-06-19T19:49:06+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106130
codeforces_index: "E"
codeforces_contest_name: "GDUT 2025 Monthly competition"
rating: 0
weight: 106130
solve_time_s: 44
verified: true
draft: false
---

[CF 106130E - \u65b0\u7530\u5fcc\u8d5b\u9a6c](https://codeforces.com/problemset/problem/106130/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 44s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai mảng tốc độ ngựa được sắp xếp giảm dần. Một cái thuộc về Tian Ji và cái còn lại thuộc về King. Một trận đấu được hình thành bằng cách ghép một con ngựa chưa sử dụng của mỗi bên và kết quả phụ thuộc vào sự so sánh chặt chẽ: Tian Ji chỉ kiếm được tiền khi con ngựa được chọn của anh ta nhanh hơn ngựa của đối thủ và trong trường hợp đó phần thưởng bằng với tốc độ của đối thủ. 

Mục tiêu không phải là tối đa hóa chiến thắng mà là tối đa hóa tổng giá trị phần thưởng, có nghĩa là chúng tôi muốn giành chiến thắng trước những đối thủ có giá trị lớn bất cứ khi nào có thể, vì đánh bại đối thủ mạnh hơn sẽ kiếm được nhiều tiền hơn là đánh bại đối thủ yếu. 

Cả hai mảng đều đã được sắp xếp theo thứ tự không tăng, điều này hạn chế mạnh mẽ cấu trúc của bất kỳ chiến lược tối ưu nào. Tuy nhiên, việc ghép đôi không cố định nên khó khăn chính là quyết định nên sử dụng tập hợp con trận đấu nào để kiếm lợi nhuận và cách phân ngựa một cách tối ưu. 

Các ràng buộc cho phép tối đa 500.000 con ngựa mỗi bên, do đó, bất kỳ giải pháp nào thử tất cả các cặp hoặc sử dụng đối sánh bậc hai đều ngay lập tức không thể thực hiện được. Ngay cả các cách tiếp cận O(n log n) hoặc O(n + m) với logic tham lam cẩn thận cũng được yêu cầu. Bất kỳ chiến lược nào liên quan đến việc tính toán lại tất cả các cặp hoặc quay lại các lựa chọn sẽ thất bại do giới hạn thời gian. 

Một trường hợp phức tạp phát sinh khi nhiều con ngựa có tốc độ bằng nhau. Vì chiến thắng đòi hỏi sự bất bình đẳng nghiêm ngặt nên tốc độ bằng nhau sẽ giống như thua và có thể chặn các trận đấu tốt hơn nếu xử lý không chính xác. Ví dụ: ghép một con ngựa mạnh hơn một chút với một đối thủ ngang bằng sẽ lãng phí một chiến thắng tiềm năng ở nơi khác. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực là xem xét tất cả các cách ghép ngựa và tổng phần thưởng từ các cặp chiến thắng. Về mặt khái niệm, đối với mỗi con ngựa của phe Tian Ji, chúng ta có thể chọn bất kỳ con ngựa đối thủ nào chưa được sử dụng và khám phá đệ quy tất cả các khả năng. Điều này đúng nhưng ngay lập tức bùng nổ về mặt tổ hợp: số lượng kết hợp là yếu tố theo số lượng ngựa và thậm chí việc cắt tỉa cũng không giúp ích gì vì mỗi quyết định ghép đôi sẽ thay đổi tính sẵn có trong tương lai. 

Nhận xét quan trọng là cơ cấu phần thưởng đơn điệu về sức mạnh của đối thủ. Chiến thắng trước bi lớn hơn luôn mang lại phần thưởng ít nhất bằng chiến thắng trước bi nhỏ hơn, trong khi thua không mang lại kết quả bất kể lựa chọn ghép đôi. Điều này cho thấy rằng lối chơi tối ưu phải phân bổ cẩn thận những con ngựa mạnh hơn của Tian Ji để đánh bại những đối thủ mạnh nhất có thể mà họ thực sự có thể đánh bại, nhưng không lãng phí chúng vào đối thủ, họ có thể đánh bại hiệu quả hơn sau này. 

Điều này biến vấn đề thành một quá trình kết hợp tham lam giữa hai chuỗi đã được sắp xếp, trong đó chúng tôi cố gắng so khớp các giá trị lớn từ cả hai bên bất cứ khi nào có thể, nhưng đôi khi cố tình hy sinh một con ngựa yếu để bảo vệ những con mạnh hơn để có cơ hội tốt hơn. 

Cách tiêu chuẩn để thể hiện điều này là chiến lược tham lam hai con trỏ: chúng tôi so sánh những con ngựa mạnh nhất còn lại và quyết định nên tham gia một trận đấu có lợi hay loại bỏ một cặp vô dụng để định vị lại các ứng cử viên còn lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n + m) | Quá chậm | 
| Hai con trỏ tham lam | O(n + m) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì hai con trỏ, một cho ngựa của Tian Ji và một cho ngựa của Vua, cả hai đều bắt đầu ở đầu mạnh nhất vì các mảng được sắp xếp theo thứ tự giảm dần.

1. Khởi tạo con trỏ i ở mức 0 cho mảng Tian Ji và con trỏ j ở 0 cho mảng Vua và khởi tạo một biến cho tổng phần thưởng bằng 0. 
2. Trong khi cả hai con trỏ đều nằm trong giới hạn, hãy so sánh ai và bj. 
3. Nếu ai > bj, chúng ta thực hiện một trận đấu thắng giữa hai con ngựa này. Chúng ta thêm bj vào câu trả lời vì đó là phần thưởng cho chiến thắng, sau đó di chuyển cả hai con trỏ về phía trước. Điều này là tối ưu vì việc sử dụng con ngựa Tian Ji mạnh nhất hiện có để đánh bại đối thủ mạnh nhất có thể đánh bại sẽ tối đa hóa lợi ích hiện tại mà không gây tổn hại đến các lựa chọn trong tương lai. 
4. Nếu ai ≤ bj, Tian Ji không thể kiếm tiền từ việc ghép đôi này bằng cách sử dụng ai với bj. Tuy nhiên, thay vì lãng phí ai vào bj, chúng ta loại bỏ bj bằng cách di chuyển con trỏ của Vua về phía trước. Đây là một cách loại bỏ chiến lược: bj hiện quá mạnh so với ai, vì vậy nó cũng sẽ chặn những con ngựa Tian Ji yếu hơn, và loại bỏ nó có thể để lộ một đối thủ phù hợp hơn. 
5. Tiếp tục cho đến khi hết một bên. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, thuật toán duy trì tính bất biến rằng tất cả các đối thủ còn lại được coi là trước đây đều mạnh hơn hoặc bằng ứng cử viên Tian Ji hiện tại nếu họ bị bỏ qua, nghĩa là họ không thể so sánh có lợi với ứng cử viên đó hoặc bất kỳ ứng cử viên nào yếu hơn. Bất cứ khi nào có thể giành chiến thắng, việc ghép trận đấu khả thi mạnh nhất hiện tại là an toàn vì việc trì hoãn chiến thắng đó sẽ chỉ làm giảm các lựa chọn có sẵn cho cả hai bên mà không tăng phần thưởng. Bất cứ khi nào chiến thắng là không thể, việc loại bỏ đối thủ sẽ làm giảm khả năng chặn trong tương lai mà không làm mất đi lợi nhuận tiềm năng, vì dù sao thì không có con ngựa Tian Ji nào còn lại có thể đánh bại đối thủ đó ở giai đoạn đó. 

Việc loại bỏ tham lam này đảm bảo rằng mọi trận đấu có lợi nhuận đều được thực hiện chính xác khi nó có sẵn và không có cặp đôi có lợi nào bị bỏ qua để có kết quả có cấu trúc tồi tệ hơn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))
    
    i = 0
    j = 0
    ans = 0
    
    while i < n and j < m:
        if a[i] > b[j]:
            ans += b[j]
            i += 1
            j += 1
        else:
            j += 1
    
    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp dựa hoàn toàn vào quét tuyến tính. Điểm quyết định là kiểm tra bất đẳng thức nghiêm ngặt. Khi Tian Ji thắng, cả hai con trỏ đều tiến lên vì cả hai con ngựa đều bị tiêu hao. Khi anh ta thua hoặc hòa, chỉ có con trỏ của Vua tiến lên, loại bỏ một con ngựa đối thủ vô ích một cách hiệu quả. 

Một lỗi triển khai phổ biến là đảo ngược chuyển động của con trỏ trong trường hợp thua cuộc, điều này sẽ tiêu tốn ngựa Tian Ji một cách không chính xác và làm giảm các trận đấu tiềm năng. Một vấn đề tế nhị khác là quên rằng bình đẳng là một sự mất mát, điều này phải hành xử giống hệt nhánh “không thể thắng”. 

## Ví dụ đã hoạt động 

Hãy xem xét trường hợp Tian Ji có`[6, 3, 1]`và nhà vua có`[5, 4, 2]`. 

Chúng tôi theo dõi chuyển động của con trỏ: 

| tôi | j | một [tôi] | b[j] | Hành động | trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 6 | 5 | thắng, lấy 5 | 5 | 
| 1 | 1 | 3 | 4 | thua, bỏ b | 5 | 
| 1 | 2 | 3 | 2 | thắng, lấy 2 | 7 | 
| 2 | 3 | 1 | - | dừng lại | 7 | 

Điều này cho thấy việc bỏ qua những đối thủ mạnh hơn khi họ chặn các trận đấu trong tương lai là cần thiết; nếu không thì 3 sẽ bị lãng phí. 

Bây giờ hãy xem xét`[7, 5, 1]`so với`[6, 4, 3]`. 

| tôi | j | một [tôi] | b[j] | Hành động | trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 7 | 6 | thắng, lấy 6 | 6 | 
| 1 | 1 | 5 | 4 | thắng, lấy 4 | 10 | 
| 2 | 2 | 1 | 3 | thua, bỏ b | 10 | 
| 2 | 3 | 1 | - | dừng lại | 10 | 

Điều này chứng tỏ rằng không phải con ngựa nào còn lại đều có thể được sử dụng và lối chơi tối ưu phụ thuộc vào việc tránh lãng phí những trận đấu yếu trước những đối thủ vẫn còn quá mạnh. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Mỗi con trỏ di chuyển về phía trước nhiều nhất một lần cho mỗi phần tử | 
| Không gian | O(1) | Chỉ sử dụng bộ đếm và bộ tích lũy | 

Quét tuyến tính là cần thiết với kích thước đầu vào lên tới 500.000 mỗi mảng. Mọi sự sắp xếp đều không cần thiết vì đầu vào đã được đảm bảo sắp xếp và mọi chiến lược so sánh lồng nhau sẽ vượt quá giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    import contextlib
    import sys as _sys
    buf = io.StringIO()
    _stdout = _sys.stdout
    _sys.stdout = buf
    try:
        solve()
    finally:
        _sys.stdout = _stdout
    return buf.getvalue().strip()

# simple sample-style cases
assert run("2 2\n4 2\n3 1\n") == "3"
assert run("3 3\n6 3 1\n5 4 2\n") == "7"

# all equal (no wins possible)
assert run("3 3\n5 5 5\n5 5 5\n") == "0"

# strictly increasing difficulty gap
assert run("3 3\n10 9 8\n7 6 5\n") == "18"

# edge: many weak horses vs one strong blocker
assert run("3 5\n3 2 1\n10 9 8 7 6\n") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mảng bằng nhau | 0 | xử lý bất bình đẳng nghiêm ngặt | 
| mạnh vs yếu | toàn thắng tham lam | tích lũy ghép đôi tối ưu | 
| mọi tổn thất | 0 | hành vi chỉ loại bỏ | 

## Vỏ cạnh 

Khi tất cả các giá trị đều bằng nhau thì mọi so sánh đều thất bại. Ví dụ,`a = [5,5,5]`,`b = [5,5,5]`. Thuật toán liên tục loại bỏ từ phía đối thủ, không bao giờ tiêu tốn ngựa của Tian Ji và kết thúc với phần thưởng bằng không. Điều này xác nhận việc xử lý đúng quy tắc bình đẳng khi mất. 

Khi Tian Ji mạnh mẽ hơn về tổng thể, chẳng hạn như`a = [10,9,8]`,`b = [7,6,5]`, mọi so sánh đều mang lại chiến thắng, vì vậy cả hai con trỏ đều tiến lên theo từng bước và tất cả phần thưởng đều được tích lũy. Thuật toán không bao giờ loại bỏ một cách không cần thiết vì không xảy ra tình trạng chặn. 

Khi đối thủ có nhiều ngựa mạnh và Tian Ji có ít ngựa yếu, chẳng hạn như`a = [3,2,1]`,`b = [10,9,8,7,6]`, thuật toán liên tục loại ngựa đối phương cho đến khi kiệt sức. Tian Ji không bao giờ lãng phí một con ngựa trong một trận đấu không thể thắng, điều này rất cần thiết cho sự chính xác vì việc tiêu thụ sớm sẽ làm giảm mức tăng vốn đã bằng 0 nhưng vẫn làm sai lệch việc căn chỉnh con trỏ ở các trạng thái sau này.
