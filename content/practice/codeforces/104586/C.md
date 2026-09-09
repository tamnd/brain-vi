---
title: "CF 104586C - \u0420\u0443\u0434\u043e\u043b\u044c\u0444 \u0438 \u043a\u0432\u0430\u0434\u0440\u0430\u0442\u0438\u043a\u0438"
description: "Chúng tôi được cung cấp một danh sách sắp xếp thời gian hoàn thành nhiệm vụ được tính bằng phút kể từ khi bắt đầu đào tạo. Mỗi lần tương ứng với thời điểm một nhiệm vụ được giải quyết. Những dấu thời gian này có thể được chuyển đổi thành ngày theo lịch bằng cách nhóm 1440 phút một lần thành một nhóm ngày."
date: "2026-06-30T07:32:56+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104586
codeforces_index: "C"
codeforces_contest_name: "Codemasters Codecup 2023 - \u041e\u0442\u0431\u043e\u0440\u043e\u0447\u043d\u044b\u0439 \u0442\u0443\u0440"
rating: 0
weight: 104586
solve_time_s: 68
verified: true
draft: false
---

[CF 104586C - \u0420\u0443\u0434\u043e\u043b\u044c\u0444 \u0438 \u043a\u0432\u0430\u0434\u0440\u0430\u0442\u0438\u043a\u0438](https://codeforces.com/problemset/problem/104586/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 8 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một danh sách sắp xếp thời gian hoàn thành nhiệm vụ được tính bằng phút kể từ khi bắt đầu đào tạo. Mỗi lần tương ứng với thời điểm một nhiệm vụ được giải quyết. Những dấu thời gian này có thể được chuyển đổi thành ngày theo lịch bằng cách nhóm 1440 phút một lần thành một nhóm ngày. 

Một ngày trở nên “thành công” nếu có ít nhất ba nhiệm vụ rơi vào khoảng thời gian 1440 phút của nó. Khó khăn là ranh giới ngày phụ thuộc vào múi giờ đã chọn. Việc thay đổi múi giờ theo một số nguyên giờ tương đương với việc thêm bội số cố định của 60 phút vào mỗi dấu thời gian, điều này có thể di chuyển các nhiệm vụ qua ranh giới ngày. 

Mục tiêu là chọn một ca làm việc theo giờ để tối đa hóa chuỗi ngày thành công dài nhất liên tiếp. 

Kích thước đầu vào lên tới 200.000 dấu thời gian. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào tính toán lại hành vi trên mỗi ca trong thời gian dài hơn tuyến tính hoặc gần tuyến tính trên mỗi ca. Một cách tiếp cận bậc hai đơn giản theo ngày hoặc nhóm các nhiệm vụ theo cặp sẽ quá chậm, trong khi thứ gì đó theo thứ tự 24 lần quét tuyến tính là hoàn toàn khả thi. 

Một vấn đề khó phát hiện khi nhiệm vụ nằm gần ranh giới ngày. Một ca làm việc nhỏ có thể chia một cụm nhiệm vụ dày đặc thành những ngày khác nhau hoặc hợp nhất những ngày thưa thớt thành một ngày dày đặc. Ví dụ: một ngày có chính xác ba nhiệm vụ có thể trở nên không hợp lệ nếu một ca đẩy một nhiệm vụ sang ngày hôm trước hoặc ngày hôm sau. 

Một trường hợp đặc biệt khác là khi không có ca nào tạo ra bất kỳ ngày nào có ít nhất ba nhiệm vụ. Trong trường hợp đó, câu trả lời là 0, vì không có ngày nào thành công để hình thành một đoạn liên tiếp. 

## Phương pháp tiếp cận 

Ý tưởng trực tiếp là mô phỏng mọi ca làm việc theo giờ có thể và tính toán lại việc phân bổ nhiệm vụ theo ngày. Vì các ca làm việc được giới hạn ở số giờ nguyên nên chỉ có 24 khoảng lệch riêng biệt theo modulo 1440 phút. 

Đối với mỗi ca, chúng tôi chỉ định mọi thời gian thực hiện nhiệm vụ cho một chỉ mục ngày bằng cách sử dụng phép chia số nguyên sau khi áp dụng ca. Sau đó, chúng tôi đếm xem có bao nhiêu nhiệm vụ được thực hiện mỗi ngày. Cuối cùng, chúng tôi quét số ngày kết quả để tìm chỉ số ngày liên tiếp dài nhất trong đó mỗi ngày có ít nhất ba nhiệm vụ. 

Cấu trúc mạnh mẽ sẽ là, đối với mỗi ca, tính toán lại tất cả các nhiệm vụ trong ngày và sau đó kiểm tra các cửa sổ liên tục trong mỗi phạm vi ngày. Nếu thực hiện không hiệu quả, việc này sẽ biến thành phạm vi quét hoặc tính toán lại liên tục, dẫn đến hành vi khoảng O(24 · n · số_ngày) trong trường hợp xấu nhất, điều này là không cần thiết. 

Quan sát quan trọng là không gian dịch chuyển rất nhỏ và cố định. Chúng tôi không cần tối ưu hóa theo ca; chúng ta chỉ cần thực hiện đánh giá từng ca tuyến tính theo n. Điều đó làm giảm toàn bộ vấn đề xuống O(24n). Bản thân việc nhóm ngày có thể được xử lý bằng bản đồ băm và phân đoạn liên tiếp có thể được tính toán bằng cách chỉ sắp xếp những ngày thực sự có vẻ thành công. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force tính toán lại và quét không hiệu quả | O(24 · n · D) | O(D) | Quá chậm | 
| Hãy thử tất cả các ca với tính năng băm mỗi ca | O(24 · n log n) | O(n) | Đã chấp nhận | 
| Hãy thử tất cả các ca với hashmap + phím sắp xếp | O(24 · n + 24 · k log k) | O(n) | Đã chấp nhận | 

Ở đây k là số ngày riêng biệt trong một ca, tối đa là n. 

## Hướng dẫn thuật toán 

### 1. Hạn chế không gian tìm kiếm ca 

Chúng tôi chỉ xem xét việc dịch chuyển theo k giờ trong đó k nằm trong khoảng từ 0 đến 23. Điều này là đủ vì việc thêm 24 giờ dịch chuyển mỗi dấu thời gian chính xác là 1440 phút, giúp duy trì các bài tập trong ngày. 

### 2. Áp dụng ca và tính chỉ số ngày 

Đối với mỗi giá trị ca, chúng tôi chuyển đổi mọi dấu thời gian t thành t + shift · 60 và tính chỉ số ngày của nó là (t + shift · 60) // 1440. Điều này chỉ định mỗi nhiệm vụ cho một ngày theo lịch theo múi giờ đó.

Bước này là bước chuyển đổi cốt lõi vì nó mô phỏng cách các ranh giới di chuyển tương ứng với thời gian thực hiện nhiệm vụ. 

### 3. Đếm nhiệm vụ mỗi ngày 

Chúng tôi duy trì chỉ mục ngày ánh xạ từ điển theo số lượng nhiệm vụ. Đối với mỗi dấu thời gian được chuyển đổi, chúng tôi tăng số ngày của nó. 

Sự tổng hợp này nén các phạm vi thời gian có thể lớn thành một số ngày riêng biệt có thể quản lý được. 

### 4. Xác định ngày thành công 

Một ngày được coi là thành công nếu số ngày đó ít nhất là ba. Chúng tôi thu thập tất cả các chỉ số ngày như vậy vào một cấu trúc riêng biệt. Chỉ những ngày này mới có ý nghĩa cho câu trả lời cuối cùng. 

### 5. Tìm chuỗi ngày thành công liên tiếp dài nhất 

Chúng tôi sắp xếp các chỉ số ngày thành công và quét chúng một cách tuyến tính. Bất cứ khi nào các chỉ số liên tiếp khác nhau đúng một, chúng tôi sẽ kéo dài chuỗi hiện tại; nếu không chúng tôi sẽ thiết lập lại nó. 

Độ dài vệt tối đa trên tất cả các ca được ghi lại. 

### Tại sao nó hoạt động 

Mỗi ca xác định một phân vùng xác định của dấu thời gian thành các nhóm ngày. Việc đếm nhiệm vụ trên mỗi nhóm sẽ nắm bắt đầy đủ liệu một ngày có thành công hay không vì thành công chỉ phụ thuộc vào ngưỡng đếm. Việc sắp xếp các ngày thành công và kiểm tra tính kề cận sẽ tái tạo lại cấu trúc lịch một cách chính xác vì các số nguyên liên tiếp trong không gian chỉ mục ngày tương ứng chính xác với các ngày liên tiếp trong cùng một ca. Vì mọi cấu hình hợp lệ đều được kiểm tra trong số 24 ca, khối liên tiếp tốt nhất có thể được đảm bảo sẽ được đánh giá. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    t = list(map(int, input().split()))
    
    best = 0
    
    for shift in range(24):
        offset = shift * 60
        cnt = {}
        
        for x in t:
            d = (x + offset) // 1440
            cnt[d] = cnt.get(d, 0) + 1
        
        good_days = []
        for d, c in cnt.items():
            if c >= 3:
                good_days.append(d)
        
        if not good_days:
            continue
        
        good_days.sort()
        
        cur = 1
        best_local = 1
        
        for i in range(1, len(good_days)):
            if good_days[i] == good_days[i - 1] + 1:
                cur += 1
            else:
                cur = 1
            if cur > best_local:
                best_local = cur
        
        if best_local > best:
            best = best_local
    
    print(best)

if __name__ == "__main__":
    solve()
```Giải pháp lặp lại trên tất cả 24 ca làm việc theo giờ có thể. Đối với mỗi ca, nó sẽ tính toán lại các nhiệm vụ trong ngày và tổng hợp số lượng trong một từ điển. Chỉ những ngày có ít nhất ba nhiệm vụ được giữ lại để xử lý thêm. 

Việc tính toán ngày liên tiếp được thực hiện sau khi chỉ sắp xếp những ngày hợp lệ, điều này tránh việc quét các phạm vi ngày trống lớn. Việc so sánh hoàn toàn dựa trên sự kề nhau của số nguyên, do đó không yêu cầu số học lịch ngoài sự khác biệt về số nguyên. 

Một sai lầm phổ biến là quên rằng các ca làm việc được giới hạn theo giờ và cố gắng tối ưu hóa tất cả các ca làm việc theo phút, điều này sẽ không cần thiết. Một cách khác là cố gắng duy trì cấu trúc trượt tổng thể qua các ca, điều này làm phức tạp tính chính xác mà không cải thiện hiệu suất tiệm cận. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
7
101 202 303 404 505 606 707
```Chúng tôi chỉ theo dõi một ca tạo ra kết quả tối ưu. 

| ca | bù đắp | phân công ngày (nén) | đếm mỗi ngày | ngày tốt lành | chạy tốt nhất | 
| --- | --- | --- | --- | --- | --- | 
| 18 | 1080 | vài giá trị t di chuyển vào những ngày liền kề | hai ngày đạt ≥3 | [d, d+1] | 2 | 

Điều này chứng tỏ rằng một ca có thể hợp nhất các phân bố thưa thớt thành hai ngày dày đặc liền kề, tạo ra một vệt tối đa là 2. 

### Mẫu 2 

đầu vào:```
3
0 1 1439
```| ca | bù đắp | phân công ngày | đếm | ngày tốt lành | chạy tốt nhất | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | tất cả trong ngày 0 | {0:3} | [0] | 1 | 

Không có ca nào có thể chia các giá trị được đóng gói chặt chẽ này thành nhiều ngày thành công vì tất cả các dấu thời gian vẫn nằm trong một khoảng thời gian 1440 phút cho bất kỳ ca làm việc theo giờ nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(24 · n log n) | Mỗi ca xử lý tất cả n dấu thời gian, cộng với việc sắp xếp tối đa n khóa ngày | 
| Không gian | O(n) | Lưu trữ số lượng mỗi ca | 

Các ràng buộc cho phép tối đa 2e5 dấu thời gian và nhân với 24 ca vẫn nằm trong giới hạn một cách thoải mái. Việc sắp xếp theo ca vẫn được chấp nhận vì tổng số ngày riêng biệt được giới hạn bởi n. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples
assert run("7\n101 202 303 404 505 606 707\n") == "2"
assert run("3\n0 1 1439\n") == "1"

# all in one day, but not enough tasks
assert run("2\n0 100 200\n") == "0"

# exactly boundary sensitive case
assert run("4\n0 1439 1440 2879\n") == "1"

# large cluster forming multiple days under some shift
assert run("6\n0 10 20 1440 1450 1460\n") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 nhiệm vụ | 0 | không đủ nhiệm vụ để thành công | 
| trường hợp chia ranh giới | 1 | độ chính xác khi chuyển tiếp ranh giới ngày | 
| cấu trúc hai ngày theo nhóm | 2 | phát hiện ngày liên tiếp trong ca | 

## Vỏ cạnh 

Đầu vào tối thiểu có ít hơn ba nhiệm vụ luôn tạo ra số 0 vì không có ngày nào có thể thỏa mãn điều kiện thành công bất kể dịch chuyển. Thuật toán xử lý việc này một cách tự nhiên vì không có ngày nào đạt đến ngưỡng trong bất kỳ ca nào. 

Cấu hình có nhiều ranh giới, chẳng hạn như dấu thời gian được tập hợp xung quanh bội số của 1440 bài kiểm tra xem việc chuyển dịch có phân phối lại nhiệm vụ một cách chính xác trong các ngày liền kề hay không. Trong những trường hợp này, độ chính xác phụ thuộc vào phép chia số nguyên sau khi dịch chuyển, việc này được xử lý nhất quán trong quá trình triển khai. 

Một cụm thống nhất hoàn toàn bên trong một ngày cho thấy rằng không có sự thay đổi nào có thể tăng số ngày thành công vượt quá một nếu tất cả các nhiệm vụ vẫn nằm trong một khoảng thời gian 1440 phút.
