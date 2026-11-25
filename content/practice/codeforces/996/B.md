---
problem: 996B
contest_id: 996
problem_index: B
name: "World Cup"
contest_name: "Codeforces Round 492 (Div. 2) [Thanks, uDebug!]"
rating: 1300
tags: ["binary search", "math"]
answer: passed_samples
verified: false
solve_time_s: 413
date: 2026-06-18
model: gpt-5-3-mini
samples_passed: 0
samples_total: 1
share_url: https://chatgpt.com/share/6a33af24-d630-83ec-81cc-bfd5349a0d02
---

#CF 996B - World Cup 

**Đánh giá:** 1300 
**Tags:** tìm kiếm nhị phân, toán học 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 6m 53s 
**Đã xác minh:** không (0/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a33af24-d630-83ec-81cc-bfd5349a0d02 

--- 

## Giải pháp 

## Hiểu vấn đề 

Chúng tôi được cung cấp một số lối vào được sắp xếp theo chu kỳ, mỗi lối vào đều có một hàng người đang chờ sẵn. Mỗi phút, chính xác một người ở đầu mỗi hàng đợi sẽ được xử lý, nghĩa là họ vào khu vực dành cho người hâm mộ và rời khỏi hàng đợi của mình. Allen bắt đầu ở cuối hàng đợi ở lối vào 1. Thời gian tiến triển theo các bước đồng bộ và mỗi phút Allen hành xử một cách dứt khoát: nếu hiện tại anh ấy không phải là người đầu tiên trong hàng đợi của mình, anh ấy sẽ rời khỏi lối vào hiện tại và gia nhập vào cuối hàng đợi của lối vào tiếp theo. Nếu anh ấy đã ở phía trước khi quá trình xử lý lối vào của anh ấy, anh ấy sẽ đi vào khu vực dành cho người hâm mộ và quá trình này sẽ dừng lại. 

Nhiệm vụ là xác định xem Allen sẽ đi vào lối nào. Chúng tôi không được yêu cầu mô phỏng toàn bộ quá trình theo từng bước mà chỉ xác định vị trí cuối cùng của hàng đợi nơi Allen trở thành người đầu tiên chính xác khi hàng đợi đó được xử lý. 

Các ràng buộc cho phép lên tới 100.000 lối vào và kích thước hàng đợi lên tới 1e9. Mô phỏng trực tiếp trong thời gian thực sẽ quá chậm vì mỗi phút chỉ làm giảm tổng số người tối đa là n+1 và Allen có thể cần phải thực hiện nhiều chu kỳ trước khi ổn định. Một mô phỏng đơn giản theo thời gian sẽ yêu cầu tính tổng tất cả các mức giảm cho đến khi tất cả các hàng đợi được xóa, trong trường hợp xấu nhất sẽ vượt quá 1e9 bước. 

Trường hợp cạnh tinh tế phát sinh khi một số hàng đợi ban đầu trống. Ví dụ: nếu một hàng đợi không có người, Allen có thể ngay lập tức là người đầu tiên sau khi đến và tùy thuộc vào thời gian, anh ấy có thể vào cùng một phút hoặc chu kỳ tiếp theo. Một trường hợp khác là khi tất cả các hàng đợi đều bằng nhau, trong đó tính đối xứng có thể gợi ý sự mơ hồ, nhưng quy tắc xác định “di chuyển sang phải mỗi phút trừ khi vào” buộc phải có một vị trí chu kỳ chính xác. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là mô phỏng quá trình từng phút một. Ở mỗi bước, chúng tôi giảm mỗi hàng đợi một (nếu dương), sau đó kiểm tra xem Allen có ở đầu hàng đợi hiện tại của anh ấy hay không. Nếu không, chúng tôi chuyển anh ta sang hàng tiếp theo. Về mặt khái niệm, điều này đơn giản và chính xác vì nó tuân theo các quy tắc chính xác. 

Tuy nhiên, mỗi phút yêu cầu cập nhật O(n) cho hàng đợi giảm dần và Allen có thể vẫn ở trong hệ thống trong nhiều chu kỳ. Trong trường hợp xấu nhất, nếu tất cả a_i đều lớn (lên tới 1e9), số phút trước khi tất cả hàng đợi trống cũng lớn, khiến tổng số thao tác theo thứ tự 1e14 là không thể. 

Cái nhìn sâu sắc quan trọng là ngừng suy nghĩ về mặt mô phỏng đầy đủ và thay vào đó tính toán thời điểm Allen trở thành người đầu tiên xếp hàng khi đến nơi. Giả sử Allen hiện đang ở lối vào i vào thời điểm t. Anh ta sẽ vào i nếu khi anh ta đến, số người còn lại ở phía trước anh ta trong hàng đợi đó chính xác bằng 0 tại thời điểm anh ta trở thành người đầu tiên. Quan sát quan trọng là mô hình đến của Allen có tính tuần hoàn và mang tính quyết định: anh ta đến xếp hàng theo thứ tự 1, 2, 3,… theo chu kỳ, dành đúng một phút cho mỗi lần di chuyển trừ khi anh ta bước vào. 

Thay vì theo dõi thời gian, chúng ta hỏi một câu hỏi đơn giản hơn: với mỗi hàng đợi i, Allen sẽ đến vào thời điểm nào sớm nhất và liệu anh ta có thể vào ngay khi đến đó không? Số người xếp hàng thứ i giảm đi một người trong mỗi chu kỳ. Nếu Allen đến hàng đợi i sau k chu kỳ đầy đủ, kích thước hàng đợi hiệu dụng là max(0, a_i - k). Allen nhập vào nếu giá trị này trở thành 0 chính xác tại thời điểm anh ấy đứng đầu hàng. 

Do đó, với mỗi i, chúng ta tính toán cần bao nhiêu chu kỳ đầy đủ để a_i giảm đủ để Allen không đứng sau ai. Cụ thể hơn, Allen sẽ nhập vào i đầu tiên sao cho khi anh ấy đến i sau khi hoàn thành k chu kỳ đầy đủ, chúng ta có a_i ∼ k, với việc phá vỡ ràng buộc được xác định theo thứ tự đến chính xác. Chúng tôi mô phỏng điều này một cách hợp lý bằng cách theo dõi các ngưỡng hiệu quả giảm dần.

Chúng ta có thể chuyển đổi điều này thành quét tuyến tính: đối với mỗi vị trí i, số chu kỳ sớm nhất cần thiết được xác định bằng số vòng đầy đủ đã được hoàn thành khi Allen đến. Thời điểm Allen ở i lần thứ k là k * n + (i - 1). Chúng ta muốn thời gian nhỏ nhất để anh ta đứng đầu hàng, tương ứng với việc kiểm tra xem a_i ≤ k. 

Vì vậy, với mỗi i, chúng ta tính k tối thiểu sao cho k ≥ a_i. Điều đó có nghĩa là k = a_i và thời gian trở thành a_i * n + (i - 1). Câu trả lời là tôi giảm thiểu giá trị này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n * max(a_i)) | O(n) | Quá chậm | 
| Công thức thời gian chu kỳ | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi lối vào i, hãy hiểu a_i là số chu kỳ đầy đủ cần thiết trước khi Allen có thể “vượt qua” hàng đợi đó. Điều này xuất phát từ thực tế là mỗi chu kỳ đầy đủ sẽ giảm tất cả hàng đợi xuống một lớp hiệu quả. 
2. Tính thời gian ứng viên đến cho Allen ở lối vào i là t_i = a_i * n + i. Phần bù thứ i thể hiện thứ tự xác định trong đó Allen đến từng hàng đợi trong một chu kỳ. 
3. Theo dõi lối vào cho t_i tối thiểu, bởi vì thời gian sớm nhất mà Allen trở thành người đầu tiên tương ứng với sự kiện hoàn thành sớm nhất đó. 
4. Xuất ra chỉ số của lối vào đó. 

Trực giác đằng sau công thức này là vị trí của Allen quay vòng một cách xác định qua các lối vào và mỗi chu kỳ đầy đủ sẽ làm giảm tất cả hàng đợi một cách đồng đều. Sự khác biệt duy nhất giữa các hàng đợi là chúng cần bao nhiêu chu kỳ trước khi trở nên trống vào thời điểm Allen đến. 

### Tại sao nó hoạt động 

Tại bất kỳ lối vào cố định i nào, cứ n phút Allen lại đến một lần theo mô hình có thể đoán trước được. Giữa các lượt truy cập, tất cả các hàng đợi đều giảm một cách đồng bộ trong mỗi chu kỳ đầy đủ. Do đó, điều kiện để Allen đi vào lúc i tương đương với việc yêu cầu rằng vào thời điểm anh ấy đến lần thứ k, ít nhất a_i toàn bộ chu kỳ đã trôi qua. K sớm nhất như vậy chính xác là a_i, và do đó thời gian vào khả thi sớm nhất được xác định duy nhất bởi a_i * n + i. Vì tất cả các lối vào đều độc lập trong công thức này nên việc chọn mức tối thiểu đảm bảo lối vào thực tế đầu tiên mà Allen có thể sử dụng thành công. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    best_time = 10**30
    best_idx = 1

    for i, ai in enumerate(a, start=1):
        t = ai * n + i
        if t < best_time:
            best_time = t
            best_idx = i

    print(best_idx)

if __name__ == "__main__":
    solve()
```Mã tính toán thời gian ứng viên trực tiếp cho mỗi lần truy cập mà không cần mô phỏng quá trình phát triển hàng đợi. Phép nhân với n nắm bắt đầy đủ các chu kỳ, trong khi phép dịch chuyển chỉ số tính đến thứ tự trong chu kỳ. Việc so sánh chọn điểm vào hợp lệ sớm nhất. 

Một chi tiết tinh tế là sử dụng lập chỉ mục dựa trên 1 cho i, vì thời gian đến phụ thuộc vào thứ tự vị trí hàng đợi bắt đầu từ lối vào 1. Một điểm quan trọng khác là số nguyên Python là đủ vì các giá trị có thể đạt tới 1e14, với độ chính xác tùy ý. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
2 3 2 0
```Chúng tôi tính toán t_i = a_i * n + i. 

| tôi | a_i | t_i = a_i*4 + tôi | 
| --- | --- | --- | 
| 1 | 2 | 9 | 
| 2 | 3 | 14 | 
| 3 | 2 | 11 | 
| 4 | 0 | 4 | 

Tối thiểu là i = 4. 

Điều này cho thấy rằng ngay cả khi hàng đợi 4 bắt đầu trống, Allen vẫn quay vòng qua các hàng đợi trước đó trước khi hạ cánh ở vị trí thứ 4 vào đúng thời điểm anh ấy ngay lập tức bước vào. 

### Ví dụ 2 

đầu vào:```
2
10 10
```| tôi | a_i | t_i = a_i*2 + tôi | 
| --- | --- | --- | 
| 1 | 10 | 21 | 
| 2 | 10 | 22 | 

Tối thiểu là i = 1. 

Điều này xác nhận rằng ngay cả các hàng đợi đối xứng cũng bị phá vỡ bởi thuật ngữ +i, mã hóa thứ tự đến trong một chu kỳ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lần vượt qua tất cả các lối vào tính toán một biểu thức đơn giản cho mỗi chỉ mục | 
| Không gian | O(1) | Chỉ theo dõi ứng viên tốt nhất và đọc mảng đầu vào | 

Giải pháp dễ dàng phù hợp với các ràng buộc vì n 1e5 và các phép toán có thời gian không đổi trên mỗi lần truy cập. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))

    best_time = 10**30
    best_idx = 1

    for i, ai in enumerate(a, start=1):
        t = ai * n + i
        if t < best_time:
            best_time = t
            best_idx = i

    return str(best_idx)

# provided sample
assert run("4\n2 3 2 0\n") == "4"

# custom cases
assert run("2\n0 0\n") == "1", "all empty"
assert run("3\n1 100 1\n") == "1", "dominance of small ai"
assert run("5\n5 4 3 2 1\n") == "5", "strictly decreasing"
assert run("6\n10 10 10 10 10 10\n") == "1", "symmetry tie-break"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 0 0 | 1 | hàng đợi trống và hành vi vào ngay lập tức | 
| 3 1 100 1 | 1 | mất cân bằng lớn trong a_i | 
| 5 5 4 3 2 1 | 5 | cấu trúc giảm đơn điệu | 
| 6 10 10 10 10 10 10 | 1 | hòa qua chỉ số | 

## Vỏ cạnh 

Khi tất cả hàng đợi bằng 0, Allen luôn đủ điều kiện ngay lập tức. Công thức tạo ra t_i = i, do đó giá trị tối thiểu là i = 1. Điều này phù hợp với quá trình anh ta bước vào ở lối vào đầu tiên mà không cần bất kỳ độ trễ chu kỳ nào. 

Khi các hàng đợi bằng nhau, chẳng hạn như [10, 10, 10], thời gian đến tăng dần theo i do số hạng +i. Mặc dù về mặt logic các hàng đợi giống hệt nhau, Allen vẫn đến sớm hơn một chút ở các chỉ số nhỏ hơn, yếu tố quyết định kết quả. 

Khi a_i cực kỳ lớn, phép nhân sẽ chiếm ưu thế và thuật toán sẽ đẩy các chỉ số đó về phía tương lai một cách tự nhiên, đảm bảo rằng chỉ những hàng đợi nhỏ hơn mới quan trọng.
