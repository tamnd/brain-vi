---
problem: 1316A
contest_id: 1316
problem_index: A
name: "Grade Allocation"
contest_name: "CodeCraft-20 (Div. 2)"
rating: 800
tags: ["implementation"]
answer: passed_samples
verified: true
solve_time_s: 132
date: 2026-06-14
model: gpt-5-3-mini
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a2de51d-4028-83ec-b74f-af94b6e7b900
---

#CF 1316A - Phân bổ cấp bậc 

**Đánh giá:** 800 
**Thẻ:** triển khai 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 2m 12s 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a2de51d-4028-83ec-b74f-af94b6e7b900 

--- 

##Giải pháp 

## Hiểu vấn đề 

Chúng ta có một lớp học trong đó mọi học sinh đều có điểm thi là số nguyên nằm trong khoảng từ 0 đến một giá trị tối đa nào đó. Nhà trường cho phép chúng ta tự do viết lại tất cả các điểm, nhưng chúng ta phải bảo toàn tổng điểm, vì việc giữ nguyên điểm trung bình tương đương với việc giữ nguyên tổng. 

Mục tiêu của chúng tôi chỉ tập trung vào học sinh 1. Chúng tôi muốn gán cho học sinh này số điểm lớn nhất có thể sau khi phân phối lại, trong khi vẫn có thể gán điểm số nguyên hợp lệ cho những người khác, mỗi người nằm trong phạm vi từ 0 đến m và giữ tổng điểm cố định. 

Vì vậy, vấn đề trở thành một nhiệm vụ phân phối lại: chúng ta lấy một lượng “khối lượng điểm” cố định bằng tổng số ban đầu và chúng ta muốn tập trung càng nhiều càng tốt vào vị trí đầu tiên mà không vượt quá giới hạn trên m của mỗi học sinh. 

Các ràng buộc đủ nhỏ để ngay cả việc quét tuyến tính bậc hai hoặc tham lam cho mỗi trường hợp thử nghiệm cũng có thể vượt qua một cách thoải mái. Với tối đa 1000 học sinh cho mỗi trường hợp kiểm tra và 200 trường hợp kiểm tra, thậm chí O(n^2) cũng có thể chấp nhận được, trong khi O(n) hoặc O(n log n) là không đáng kể. Điều này đã gợi ý rằng giải pháp không nên liên quan đến các cấu trúc tối ưu hóa hoặc tìm kiếm phức tạp. 

Một trường hợp cạnh tinh tế phát sinh khi học sinh đầu tiên đã ở gần m nhưng phần còn lại của mảng quá nhỏ nên về mặt lý thuyết, việc phân phối lại có thể đẩy nó lên cao hơn. Tuy nhiên, hardcap m chặn mọi nỗ lực như vậy. Một trường hợp khó khăn khác là khi tất cả học sinh đều có điểm bằng nhau; trong trường hợp đó, việc phân phối lại không làm thay đổi bất cứ điều gì, và câu trả lời chỉ đơn giản là m nếu tổng số tiền cho phép tập trung mọi thứ vào một học sinh. 

Một trực giác sai lầm phổ biến là nghĩ rằng chúng ta luôn có thể đặt học sinh 1 thành m. Điều này không thành công khi tổng số tiền quá nhỏ. Ví dụ: nếu n = 4, m = 10 và tổng là 6, chúng tôi không thể gán 10 cho bất kỳ học sinh nào vì chúng tôi sẽ cần ít nhất 10 đơn vị tổng điểm. 

## Phương pháp tiếp cận 

Cách mạnh mẽ nhất để suy nghĩ về vấn đề này là mô phỏng sự phân phối lại. Chúng tôi thử tăng điểm của học sinh này từng bước một và kiểm tra xem tổng số tiền còn lại có thể chia cho các học sinh khác mà không vượt quá m hoặc xuống dưới 0 hay không. Đối với mỗi giá trị ứng cử viên x cho học sinh 1, chúng tôi kiểm tra tính khả thi bằng cách xác minh xem tổng S - x còn lại có thể được chia thành n - 1 giá trị trong khoảng từ 0 đến m hay không. Điều này đúng nhưng không hiệu quả nếu được thực hiện một cách đơn giản, bởi vì với mỗi ứng cử viên x, chúng ta có thể quét hoặc tính toán lại các điều kiện khả thi nhiều lần. 

Quan sát quan trọng là các ràng buộc thực sự duy nhất là toàn cục: tổng là cố định và mỗi giá trị riêng lẻ được giới hạn độc lập. Không có sự ghép nối giữa các sinh viên ngoài giới hạn tổng. Điều đó có nghĩa là tính khả thi được xác định hoàn toàn bằng việc số tiền còn lại có phù hợp với năng lực của n - 1 học sinh còn lại hay không. 

Mỗi học sinh đó có thể chứa tối đa m, nên tổng sức chứa của họ là (n - 1) * m. Điều đó đưa ra một điều kiện khả thi đơn giản. Nếu gán x cho sinh viên 1 thì tổng còn lại S - x phải thỏa mãn 0 ≤ S - x ≤ (n - 1) * m. Điều này biến bài toán thành một bất đẳng thức trực tiếp ràng buộc trên x. 

Do đó, chúng tôi đang tối đa hóa x theo hai ràng buộc: nó không thể vượt quá m và nó không thể vượt quá S trừ đi không gian cần thiết tối thiểu cho những thứ khác. Vì những cái khác có thể lấy tối đa (n - 1) * m, x khả thi tối đa chỉ đơn giản là S - 0, nhưng được kẹp để phần còn lại vẫn vừa. 

Kết quả cuối cùng trở thành một phép tính trực tiếp thay vì tìm kiếm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Phân phối lại vũ phu | O(n * m) hoặc tệ hơn | O(n) | Quá chậm/không cần thiết | 
| Giới hạn dựa trên tổng tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Tính tổng S của tất cả điểm của học sinh. Tổng này là cố định vì việc phân phối lại bảo toàn mức trung bình. 
2. Nhận biết sau khi gán giá trị x cho học sinh 1 thì tổng còn lại sẽ là S - x. 
3. Còn lại n - 1 học sinh, mỗi em có thể chứa tối đa m, nên tổng dung lượng lưu trữ của các em là (n - 1) * m. 
4. Để lựa chọn x hợp lệ thì tổng còn lại phải thỏa mãn S - x ≤ (n - 1) * m. Sắp xếp lại sẽ có x ≥ S - (n - 1) * m. 
5. Đồng thời yêu cầu x không được vượt quá m vì điểm của mỗi cá nhân đều bị giới hạn. 
6. Vì chúng tôi muốn x tối đa có thể, nên chúng tôi lấy x = min(m, S), nhưng đảm bảo tính khả thi với những người khác đã được thỏa mãn vì S - x tự động vừa với dung lượng còn lại khi x được chọn càng lớn càng tốt dưới giới hạn. 

### Tại sao nó hoạt động 

Bất biến chính là tính khả thi chỉ phụ thuộc vào tổng năng lực còn lại chứ không phụ thuộc vào sự phân bổ giữa từng học sinh. Khi học sinh 1 được cố định thành x, phần còn lại của bài toán chuyển sang kiểm tra xem (n - 1) khe có dung lượng m có thể hấp thụ chính xác S - x đơn vị hay không. Vì các vị trí đó độc lập và giống nhau về dung lượng nên mọi phần còn lại trong phạm vi luôn có thể được phân phối một cách tham lam mà không vi phạm giới hạn. Điều này loại bỏ mọi nhu cầu về phân công có cấu trúc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    
    s = sum(a)
    
    # maximum possible for student 1 is bounded by:
    # 1) cannot exceed m
    # 2) cannot exceed total sum (we must leave non-negative for others)
    print(min(m, s))
```Sau khi đọc dữ liệu đầu vào, chúng tôi tính tổng tổng của tất cả các điểm. Câu trả lời sau đó bị ràng buộc bởi hai giới hạn độc lập: học sinh 1 không thể vượt quá m và không thể vượt quá tổng số tiền có sẵn S. Bất kỳ số tiền còn lại nào cũng luôn có thể được gán cho n - 1 học sinh khác vì năng lực tổng hợp của họ ít nhất là S - m khi x được chọn tối ưu và các giới hạn đảm bảo không xảy ra vi phạm. 

Việc triển khai tránh mọi logic mô phỏng hoặc phân phối lại. Tính đúng đắn hoàn toàn đến từ việc quy giản vấn đề thành luận cứ về năng lực toàn cầu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào: 

n = 4, m = 10 

a = [1, 2, 3, 4] 

Tổng số tiền S = 10 

Chúng tôi tính x = phút (10, 10) = 10. 

| Bước | S | m | x | 
| --- | --- | --- | --- | 
| tính tổng | 10 | 10 | - | 
| áp dụng ràng buộc | 10 | 10 | 10 | 

Tổng còn lại trở thành 0, tương đương với 3 học sinh. Điều này xác nhận rằng có thể tập trung hoàn toàn. 

### Ví dụ 2 

đầu vào: 

n = 4, m = 5 

a = [1, 2, 3, 4] 

Tổng số tiền S = 10 

Chúng tôi tính x = min(5, 10) = 5. 

| Bước | S | m | x | 
| --- | --- | --- | --- | 
| tính tổng | 10 | 5 | - | 
| áp dụng ràng buộc | 10 | 5 | 5 | 

Tổng còn lại là 5, ba học sinh có thể dễ dàng tiếp thu vì tổng sức chứa của họ là 15. 

Điều này cho thấy giới hạn m là ràng buộc hoạt động ở đây chứ không phải là tổng số tiền. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | tổng hợp mảng chiếm ưu thế | 
| Không gian | O(1) thêm | chỉ có một khoản tiền được lưu trữ | 

Các ràng buộc cho phép tổng số phần tử lên tới 200k trong tất cả các thử nghiệm, do đó, một lần quét tuyến tính cho mỗi trường hợp thử nghiệm có thể dễ dàng đủ nhanh trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n, m = map(int, input().split())
        a = list(map(int, input().split()))
        out.append(str(min(m, sum(a))))
    return "\n".join(out)

# provided samples
assert run("""2
4 10
1 2 3 4
4 5
1 2 3 4
""") == "10\n5"

# minimum case
assert run("""1
1 100
42
""") == "42"

# all equal small values
assert run("""1
5 10
2 2 2 2 2
""") == "10"

# max cap dominates
assert run("""1
3 5
10 10 10
""") == "5"

# tight sum case
assert run("""1
4 3
1 1 1 1
""") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| sinh viên độc thân | 42 | n = vỏ 1 cạnh | 
| giá trị bằng nhau | 10 | tái phân phối tính trung lập | 
| giới hạn giá trị lớn | 5 | m hạn chế sự thống trị | 
| tổng chặt chẽ | 3 | kịch bản giới hạn tổng | 

## Vỏ cạnh 

Khi n = 1, không có học sinh nào khác để phân bổ, do đó hạn chế duy nhất là điểm tối đa cho phép m. Thuật toán tính toán min(m, S) và vì S bằng a1 nên kết quả chính xác sẽ trở thành a1, đây là phép gán khả thi duy nhất. 

Khi tất cả học sinh đã có giá trị bằng m thì tổng là n * m. Công thức trả về min(m, n * m) = m, phù hợp với thực tế là học sinh 1 không thể vượt quá giới hạn cho mỗi học sinh mặc dù tổng khối lượng đủ lớn. 

Khi tổng nhỏ hơn m thì đáp án chính xác là tổng. Điều này tương ứng với việc tập trung tất cả khối điểm có sẵn vào học sinh 1 trong khi đặt tất cả những điểm khác về 0, điều này vẫn hợp lệ vì 0 nằm trong giới hạn. 

Mỗi trường hợp này cho thấy giải pháp chỉ phụ thuộc vào năng lực toàn cầu chứ không phụ thuộc vào sự sắp xếp lại của từng cá nhân, xác nhận rằng không có cấu trúc ẩn nào ảnh hưởng đến kết quả.
