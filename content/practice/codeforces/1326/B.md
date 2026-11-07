---
problem: 1326B
contest_id: 1326
problem_index: B
name: "Maximums"
contest_name: "Codeforces Global Round 7"
rating: 900
tags: ["implementation", "math"]
answer: passed_samples
verified: true
solve_time_s: 223
date: 2026-06-14
model: gpt-5-3-mini
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a2df5cc-122c-83ec-9663-528b7493c968
---

# CF 1326B - Tối đa 

**Xếp hạng:** 900 
**Tags:** thực hiện, toán học 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 3m 43s 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a2df5cc-122c-83ec-9663-528b7493c968 

--- 

##Giải pháp 

## Hiểu vấn đề 

Chúng tôi được cung cấp một phiên bản chuyển đổi của một mảng không xác định. Mảng ban đầu chứa các số nguyên không âm và đối với mỗi vị trí, một giá trị khác được lấy từ giá trị tối đa của tất cả các phần tử trước đó. Mức tối đa đó sau đó được trừ khỏi phần tử hiện tại để tạo ra mảng mà chúng ta đã cho. 

Cụ thể hơn, mỗi vị trí trong mảng ẩn bị ảnh hưởng bởi giá trị tối đa nhìn thấy cho đến nay ở bên trái của nó. Phép biến đổi ẩn các giá trị ban đầu bằng cách trừ liên tục tiền tố tối đa đang chạy này. 

Nhiệm vụ là xây dựng lại bất kỳ mảng ban đầu hợp lệ nào có thể tạo ra mảng được chuyển đổi đã cho. Mặc dù có thể có nhiều cách giải thích trung gian, nhưng vấn đề đảm bảo rằng câu trả lời là duy nhất. 

Các ràng buộc cho phép tối đa 200.000 phần tử, điều này ngay lập tức loại trừ mọi hoạt động tái cấu trúc bậc hai thử mọi khả năng cho từng vị trí. Bất kỳ giải pháp nào cũng phải hoạt động theo thời gian tuyến tính hoặc tệ nhất là thời gian gần tuyến tính, vì ngay cả O(n log n) cũng đã được chấp nhận nhưng không cần thiết ở đây. 

Một điểm tinh tế là sự chuyển đổi không hoàn toàn mang tính cục bộ. Mỗi giá trị phụ thuộc vào tiền tố tối đa, tiền tố này phụ thuộc vào tất cả các giá trị được xây dựng lại trước đó. Điều này giới thiệu một chuỗi phụ thuộc có thể dễ dàng phá vỡ các nỗ lực ngây thơ cố gắng gán các giá trị một cách tham lam mà không theo dõi cẩn thận cách tiền tố tối đa phát triển. 

Một ý tưởng sai lầm phổ biến là cho rằng chúng ta có thể xây dựng lại từng phần tử một cách độc lập bằng cách đoán giá trị tối đa của tiền tố. Điều này không thành công vì bản thân tiền tố tối đa phụ thuộc vào các giá trị được xây dựng lại trước đó và bất kỳ sự không nhất quán nào sớm được truyền về phía trước và làm mất hiệu lực của việc tái thiết sau này. 

Ví dụ: nếu chúng tôi giả định không chính xác mức tối đa của tiền tố vẫn ở mức nhỏ, chúng tôi có thể xây dựng lại các giá trị vi phạm các ràng buộc sau này trong đó các khác biệt được cung cấp ngụ ý giá trị ẩn lớn hơn nhiều. Sự ghép nối giữa các vị trí làm cho việc tái thiết độc lập không thể thực hiện được. 

## Phương pháp tiếp cận 

Chiến lược brute-force sẽ cố gắng xây dựng lại mảng ban đầu bằng cách đoán chuỗi tối đa tiền tố ẩn. Tại mỗi chỉ mục, chúng ta có thể thử tất cả các giá trị tối đa có thể có trước đó phù hợp với các vị trí trước đó và xác minh xem liệu kết quả tái tạo có khớp với mảng được chuyển đổi đã cho hay không. Điều này nhanh chóng bùng nổ vì mỗi vị trí có khả năng phân nhánh thành nhiều trạng thái tối đa có thể có và trên n vị trí, điều này sẽ trở thành cấp số nhân trong trường hợp xấu nhất. 

Quan sát quan trọng là phép biến đổi xác định mối quan hệ xác định giữa mảng ẩn và tiền tố tối đa của nó. Nếu chúng ta xử lý mảng từ trái sang phải, chúng ta có thể xây dựng lại các giá trị ban đầu trong khi vẫn duy trì mức tối đa tiền tố hiện tại của tiền tố được xây dựng lại. 

Tại chỉ mục i, nếu chúng ta đã biết tiền tố tối đa của a₁ đến aᵢ₋₁, thì định nghĩa trực tiếp cho chúng ta xᵢ. Vì bᵢ = aᵢ − xᵢ nên chúng ta có thể viết lại aᵢ = bᵢ + xᵢ. Điều này có nghĩa là khi biết xᵢ thì aᵢ sẽ được sửa ngay lập tức. Sau khi tính aᵢ, chúng ta cập nhật tiền tố tối đa cho các bước tiếp theo. 

Toàn bộ quá trình tái thiết giảm xuống mức duy trì mức hoạt động tối đa và áp dụng công thức trực tiếp ở mỗi bước. Bài toán trở thành một mô phỏng đơn giản của định nghĩa ngược lại ban đầu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | hàm mũ | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) bổ sung (ngoài đầu ra) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng lại mảng từ trái sang phải trong khi duy trì mức tối đa của tất cả các giá trị được xây dựng lại trước đó.

1. Bắt đầu với tiền tố trống, trong đó giá trị tối đa nhìn thấy cho đến nay là 0. Điều này phù hợp với định nghĩa rằng tiền tố tối đa đầu tiên luôn bằng 0. 
2. Với mỗi chỉ số i từ 1 đến n, hãy tính tiền tố tối đa hiện tại trước khi đặt aᵢ. Giá trị này chính xác là max(a₁, ..., aᵢ₋₁), mà chúng tôi duy trì dưới dạng biến đang chạy. 
3. Tính aᵢ bằng quan hệ trực tiếp aᵢ = bᵢ + current_max. Điều này hiệu quả vì bᵢ được xác định là chênh lệch giữa giá trị thực và giá trị tiền tố tối đa tại vị trí đó. 
4. Sau khi tính aᵢ, cập nhật giá trị tối đa đang chạy lên max(current_max, aᵢ). Điều này đảm bảo rằng các vị trí trong tương lai sử dụng tiền tố tối đa chính xác. 
5. Lặp lại cho đến khi tất cả các giá trị được xây dựng lại. 

Logic không bao giờ cần quay lại vì mỗi bước sẽ sửa cả giá trị hiện tại và trạng thái tương lai một cách xác định. 

### Tại sao nó hoạt động 

Tại mọi vị trí i, thuật toán duy trì bất biến current_max bằng max(a₁, ..., aᵢ₋₁). Vì aᵢ được tính bằng bᵢ cộng chính xác với giá trị này nên nó thỏa mãn phương trình xác định của phép biến đổi. Cập nhật mức tối đa sẽ giữ nguyên bất biến cho lần lặp tiếp theo. Vì mỗi bước thực thi ràng buộc chính xác được sử dụng để tạo bᵢ nên mảng được xây dựng lại là giải pháp hợp lệ duy nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
b = list(map(int, input().split()))

a = [0] * n

cur_max = 0
for i in range(n):
    a[i] = b[i] + cur_max
    if a[i] > cur_max:
        cur_max = a[i]

print(*a)
```Giải pháp tuân theo việc tái thiết trực tiếp từ định nghĩa. Biến`cur_max`theo dõi tiền tố tối đa của tiền tố được xây dựng lại. Đối với mỗi vị trí, trước tiên chúng tôi tính giá trị ban đầu ẩn bằng cách sử dụng mối quan hệ nghịch đảo giữa`a[i]`,`b[i]`và tiền tố tối đa. Sau đó, chúng tôi cập nhật mức tối đa đang chạy để đảm bảo tính chính xác cho các chỉ số tiếp theo. 

Một lỗi thường gặp là cập nhật giá trị tối đa trước khi tính toán`a[i]`. Điều đó đảo ngược sự phụ thuộc và tạo ra kết quả không chính xác vì`b[i]`được xác định bằng cách sử dụng mức tối đa trước đó, không phải mức tối đa được cập nhật. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
0 1 1 -2 1
```Chúng tôi xây dựng lại từng bước: 

| tôi | b[i] | cur_max trước | a[i] = b[i] + cur_max | cur_max sau | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 0 | 0 | 0 | 
| 2 | 1 | 0 | 1 | 1 | 
| 3 | 1 | 1 | 2 | 2 | 
| 4 | -2 | 2 | 0 | 2 | 
| 5 | 1 | 2 | 3 | 3 | 

Đầu ra:```
0 1 2 0 3
```Điều này xác nhận rằng ngay cả khi các giá trị trở thành âm trong b, mảng được xây dựng lại vẫn không âm do độ lệch tối đa đang chạy. 

### Ví dụ 2 

đầu vào:```
3
1000 999999000 -1000000000
```| tôi | b[i] | cur_max trước | một [tôi] | cur_max sau | 
| --- | --- | --- | --- | --- | 
| 1 | 1000 | 0 | 1000 | 1000 | 
| 2 | 999999000 | 1000 | 1000000000 | 1000000000 | 
| 3 | -1000000000 | 1000000000 | 0 | 1000000000 | 

Đầu ra:```
1000 1000000000 0
```Điều này cho thấy mức độ tích lũy cực đại tiền tố lớn và cách các giá trị b âm tương ứng với các giá trị dưới mức tối đa đang chạy. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần tử được xử lý một lần với công việc liên tục | 
| Không gian | O(1) thêm | Chỉ duy trì mức tối đa đang chạy bên cạnh mảng đầu ra | 

Thuật toán có kích thước tuyến tính của mảng, thỏa mãn thoải mái ràng buộc lên tới 200.000 phần tử trong giới hạn thời gian 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n = int(input())
    b = list(map(int, input().split()))

    a = [0] * n
    cur_max = 0

    for i in range(n):
        a[i] = b[i] + cur_max
        cur_max = max(cur_max, a[i])

    return " ".join(map(str, a))

# provided sample
assert run("5\n0 1 1 -2 1\n") == "0 1 2 0 3"

# minimum size
assert run("3\n0 0 0\n") == "0 0 0"

# strictly increasing hidden prefix
assert run("4\n0 1 2 3\n") == "0 1 2 3"

# large negative drop
assert run("3\n1000000000 -1000000000 -1000000000\n") == "1000000000 0 0"

# alternating pattern
assert run("5\n0 5 -3 10 -2\n") == "0 5 2 12 10"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 0 0 0 | 0 0 0 | độ ổn định hoàn toàn bằng không | 
| 0 1 2 3 mẫu | 0 1 2 3 | tăng trưởng tiền tố đơn điệu | 
| giọt âm lớn | 0 0 0 sau cao điểm | xử lý giá trị b âm | 
| giá trị xen kẽ | tái thiết nhất quán | cập nhật tối đa năng động | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi tất cả các giá trị b bằng 0. Đối với đầu vào:```
3
0 0 0
```Thuật toán bắt đầu với cur_max = 0. Mỗi bước tính a[i] = 0 và cur_max không bao giờ thay đổi. Đầu ra vẫn giữ nguyên tất cả các số 0, phù hợp với việc tái cấu trúc nhất quán duy nhất. 

Một trường hợp khác là khi b chứa các giá trị âm lớn sau một đỉnh lớn. Ví dụ:```
3
1000000000 -1000000000 -1000000000
```Tại i = 1, cur_max trở thành 1000000000. Tại i = 2 và i = 3, a[i] được tính toán mỗi lần trở thành 0, vì chúng ta trừ đi tiền tố lớn nhất. Sau đó, tiền tố tối đa không thay đổi và việc xây dựng lại vẫn nhất quán với định nghĩa xᵢ là tiền tố tối đa trước mỗi vị trí.
