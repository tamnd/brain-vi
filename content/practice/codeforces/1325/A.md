---
problem: 1325A
contest_id: 1325
problem_index: A
name: "EhAb AnD gCd"
contest_name: "Codeforces Round 628 (Div. 2)"
rating: 800
tags: ["constructive algorithms", "greedy", "number theory"]
answer: passed_samples
verified: false
solve_time_s: 294
date: 2026-06-14
model: gpt-5-3-mini
samples_passed: 0
samples_total: 1
share_url: https://chatgpt.com/share/6a2df29d-d044-83ec-afec-0481b1811e79
---

#CF 1325A - EhAb VàD gCd 

**Đánh giá:** 800 
**Tags:** thuật toán xây dựng, tham lam, lý thuyết số 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 4 phút 54 giây 
**Đã xác minh:** không (0/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a2df29d-d044-83ec-afec-0481b1811e79 

--- 

##Giải pháp 

## Hiểu vấn đề 

Chúng ta được cấp một số x và với mỗi truy vấn, chúng ta phải xây dựng hai số nguyên dương a và b sao cho khi tính ước chung lớn nhất và bội số chung nhỏ nhất của chúng thì tổng của hai giá trị đó chính xác là x. 

Thay vì coi đây là một bài toán nhận dạng số học, sẽ hữu ích nếu xem nó như một nhiệm vụ xây dựng: chúng ta đang cố gắng chia x thành hai phần tương tác xuất phát từ cấu trúc của một cặp số. Gcd nắm bắt cấu trúc nhân tố chung của chúng, trong khi lcm nắm bắt khoảng cách nhân kết hợp của chúng kéo dài bao xa. 

Các ràng buộc nhỏ về số lượng trường hợp thử nghiệm, nhưng bản thân x có thể lớn tới 10^9. Điều đó ngay lập tức loại trừ bất kỳ cách tiếp cận nào cố gắng tìm kiếm trên tất cả các cặp a và b hoặc thậm chí tất cả các ứng cử viên gcd cho mỗi trường hợp thử nghiệm. Quét bậc hai hoặc thậm chí tuyến tính cho mỗi trường hợp thử nghiệm sẽ quá chậm trong trường hợp xấu nhất. 

Một điểm tinh tế là gcd và lcm được liên kết chặt chẽ với nhau. Nhiều nỗ lực ngây thơ thử các cặp ngẫu nhiên hoặc cố định một giá trị và rút ra giá trị kia, nhưng những cách tiếp cận đó thường thất bại vì việc thay đổi a và b sẽ thay đổi đồng thời cả gcd và lcm theo cách phi tuyến tính. Một kiểu lỗi phổ biến là giả định rằng việc chọn một gcd nhỏ sẽ tự động làm cho lcm hoạt động có thể dự đoán được, điều này không đúng trừ khi các con số được xây dựng cẩn thận. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: lặp qua tất cả các cặp (a, b) cho đến x, tính gcd(a, b) và lcm(a, b) và kiểm tra xem tổng của chúng có bằng x hay không. Điều này đúng vì nó làm cạn kiệt toàn bộ không gian tìm kiếm. Tuy nhiên, số lượng cặp theo thứ tự x^2, lên tới 10^18 thao tác trong trường hợp xấu nhất. Ngay cả việc tối ưu hóa tính đối xứng hoặc giới hạn cũng không khiến điều này trở nên khả thi. 

Cái nhìn sâu sắc quan trọng là ngừng cố gắng kiểm soát trực tiếp cả gcd và lcm một cách độc lập. Thay vào đó, chúng ta khai thác một đẳng thức chuẩn: nếu chúng ta viết a = g·u và b = g·v trong đó gcd(u, v) = 1, thì gcd(a, b) = g và lcm(a, b) = g·u·v. Biểu thức trở thành g + g·u·v = g(1 + u·v). Điều này cho thấy toàn bộ cấu trúc phụ thuộc vào thừa số g và cặp nguyên tố cùng nhau u và v. 

Bây giờ chúng ta muốn g(1 + u·v) = x. Cách đơn giản nhất để thỏa mãn điều này là chọn g = 1. Cách đó loại bỏ tất cả độ phức tạp của gcd và giảm điều kiện xuống 1 + u·v = x, hoặc u·v = x − 1. Chúng ta luôn có thể thỏa mãn điều này bằng cách chọn u = 1 và v = x − 1, hai số nguyên tố cùng nhau tầm thường. Điều này cho ra a = 1 và b = x − 1, luôn đúng. 

Tuyên bố vấn đề cũng chấp nhận bất kỳ cặp hợp lệ nào và mẫu bao gồm một cấu trúc hợp lệ khác cho x = 14, do đó tồn tại nhiều giải pháp. Tuy nhiên, việc xây dựng đơn giản nhất đã đáp ứng được mọi ràng buộc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(x^2) | O(1) | Quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng câu trả lời độc lập cho từng trường hợp thử nghiệm. 

1. Đọc x. 
2. Đặt a = 1. 
3. Đặt b = x − 1. 
4. Kết quả a và b. 

Lý do chọn a = 1 là hợp lệ vì nó buộc gcd(1, b) = 1 với bất kỳ b nào. Điều này đơn giản hóa hoàn toàn sự tương tác giữa gcd và lcm. 

Lcm của 1 và b luôn bằng b, vì 1 không có thừa số nguyên tố nào với bất kỳ số nào và chia hết mọi thứ. Vì vậy biểu thức gcd(a, b) + lcm(a, b) trở thành 1 + b. Đặt b = x − 1 làm cho tổng chính xác là x. 

### Tại sao nó hoạt động 

Tính đúng đắn dựa trên hai thuộc tính bất biến: gcd(1, b) luôn là 1 và lcm(1, b) luôn là b. Những giá trị này đúng với mọi số nguyên dương b. Khi những điều này được cố định, biểu thức sẽ thu gọn thành một phương trình tuyến tính trong b và chúng ta giải trực tiếp nó mà không có bất kỳ tương tác nào giữa hai hàm. Không có ràng buộc ẩn vì cả gcd và lcm đều hoạt động mang tính xác định khi có sự hiện diện của 1. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    x = int(input())
    print(1, x - 1)
```Mã phản ánh chính xác việc xây dựng. Đối với mỗi trường hợp kiểm thử, nó đọc x và xuất ra cặp (1, x−1). Không có điều kiện biên bên trong vòng lặp vì x được đảm bảo ít nhất là 2, do đó x−1 luôn dương. 

Một chi tiết triển khai tinh tế là chúng tôi không bao giờ tính toán gcd hoặc lcm một cách rõ ràng. Bất kỳ nỗ lực nào để làm như vậy đều không cần thiết và sẽ chỉ gây ra chi phí chung. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi hai đầu vào, bao gồm cả các mẫu. 

### Ví dụ 1 

Đầu vào x = 2 

| Bước | một | b | gcd(a,b) | lcm(a,b) | tổng hợp | 
| --- | --- | --- | --- | --- | --- | 
| xây dựng | 1 | 1 | 1 | 1 | 2 | 

Việc xây dựng tạo ra (1, 1). Điều này chứng tỏ trường hợp biên trong đó x là tối thiểu. Ngay cả trong trường hợp cực đoan này, x − 1 vẫn đúng và dương. 

### Ví dụ 2 

Đầu vào x = 14 

| Bước | một | b | gcd(a,b) | lcm(a,b) | tổng hợp | 
| --- | --- | --- | --- | --- | --- | 
| xây dựng | 1 | 13 | 1 | 13 | 14 | 

Điều này cho thấy hành vi chung cho các giá trị lớn hơn. gcd vẫn cố định ở mức 1, trong khi lcm theo sau b trực tiếp. 

Những dấu vết này xác nhận rằng việc xây dựng không phụ thuộc vào bất kỳ tính chất đặc biệt nào của x ngoài tính dương. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Một phép tính thời gian không đổi cho mỗi trường hợp thử nghiệm | 
| Không gian | O(1) | Không có bộ nhớ bổ sung ngoài các biến | 

Giải pháp này chia tỷ lệ tuyến tính với số lượng trường hợp thử nghiệm, không đáng kể theo ràng buộc t ≤ 100. Độ lớn của x không liên quan vì không thực hiện phép lặp nào trên giá trị của nó. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        x = int(input())
        out.append(f"{1} {x-1}")
    return "\n".join(out) + "\n"

# provided samples
assert run("2\n2\n14\n") == "1 1\n1 13\n", "sample test"

# custom cases
assert run("1\n2\n") == "1 1\n", "minimum x"
assert run("1\n1000000000\n") == "1 999999999\n", "maximum x"
assert run("3\n3\n4\n5\n") == "1 2\n1 3\n1 4\n", "small consecutive values"
assert run("1\n10\n") == "1 9\n", "mid-range check"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| x = 2 | 1 1 | ranh giới tối thiểu | 
| x = 10^9 | 1 999999999 | hạn chế tối đa | 
| giá trị x liên tiếp | 1 x−1 mỗi cái | tính nhất quán giữa các trường hợp | 
| x = 10 | 1 9 | tính đúng đắn chung | 

## Vỏ cạnh 

Trường hợp cạnh có ý nghĩa duy nhất là x nhỏ nhất có thể, x = 2. Trong trường hợp này, thuật toán tạo ra a = 1 và b = 1. Tính toán trực tiếp cho gcd(1, 1) = 1 và lcm(1, 1) = 1, do đó tổng là 2 theo yêu cầu. Cấu trúc không bị phá vỡ vì x − 1 vẫn dương. 

Đối với bất kỳ x lớn hơn nào, không có gì thay đổi về mặt cấu trúc. gcd vẫn là 1 bất kể b và lcm vẫn chính xác là b, do đó đầu ra luôn thỏa mãn phương trình cần thiết mà không cần xử lý đặc biệt hoặc logic phân nhánh.
