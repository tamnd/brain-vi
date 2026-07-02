---
title: "CF 104235A - \u041a\u0440\u0430\u0441\u0438\u0432\u043e\u0435 \u043e\u043a\u043d\u043e"
description: "Chúng ta có một cửa sổ hình chữ nhật có chiều cao H và chiều rộng W. Hai vết cắt được thực hiện. Đầu tiên, chúng ta chọn một đường cắt dọc tại một số vị trí nguyên A, chia cửa sổ thành hình chữ nhật bên trái có chiều rộng A và hình chữ nhật bên phải có chiều rộng W - A."
date: "2026-07-01T23:30:06+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104235
codeforces_index: "A"
codeforces_contest_name: "2022-2023 Olympiad Cognitive Technologies, Final Round"
rating: 0
weight: 104235
solve_time_s: 83
verified: true
draft: false
---

[CF 104235A - \u041a\u0440\u0430\u0441\u0438\u0432\u043e\u0435 \u043e\u043a\u043d\u043e](https://codeforces.com/problemset/problem/104235/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 23s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cửa sổ hình chữ nhật có chiều cao`H`và chiều rộng`W`. Hai vết cắt được thực hiện. 

Đầu tiên, chúng ta chọn một đường cắt dọc ở một số vị trí nguyên`A`, chia cửa sổ thành hình chữ nhật bên trái có chiều rộng`A`và một hình chữ nhật bên phải có chiều rộng`W - A`. 

Thứ hai, chỉ bên trong hình chữ nhật bên phải, chúng ta chọn đường cắt ngang ở độ cao`B`, chia nó thành một hình chữ nhật có chiều cao trên và dưới`B`Và`H - B`. 

Điều này tạo ra chính xác ba hình chữ nhật: 

phần bên trái, phần trên bên phải và phần dưới cùng bên phải. Diện tích của họ là`S1 = H * A`,`S2 = B * (W - A)`,`S3 = (H - B) * (W - A)`. 

Mục tiêu là chọn số nguyên`A`Và`B`sao cho diện tích nhỏ nhất trong ba diện tích này càng lớn càng tốt. 

Các ràng buộc rất nhỏ: cả hai chiều nhiều nhất là 1000. Điều đó đã gợi ý rằng ngay cả một`O(HW)`hoặc`O(W)`mỗi phương pháp thử nghiệm là hoàn toàn an toàn, trong khi mọi thứ hình khối đều không cần thiết. 

Một sai lầm ngây thơ là thử tất cả`(A, B)`cặp trực tiếp. Đó nhiều nhất là một triệu trạng thái, vẫn vượt qua, nhưng nó ẩn cấu trúc và có thể dẫn đến suy luận không chính xác nếu người ta cố gắng tối ưu hóa không chính xác. 

Một cạm bẫy tinh vi hơn xuất hiện khi cố gắng xử lý ba lĩnh vực này một cách độc lập. Ví dụ: người ta có thể cố gắng tối đa hóa từng khu vực một cách riêng biệt hoặc cân bằng cả ba khu vực cùng một lúc. Điều đó thất bại vì`S2`Và`S3`được ghép nối thông qua cùng một yếu tố`(W - A)`và đối xứng ở`B`. 

Ngoài ra còn có một ràng buộc cạnh ẩn:`A`phải có ít nhất 1 và nhiều nhất`W-1`, tương tự`B`phải ở giữa`1`Và`H-1`. Điều này ngăn chặn các trường hợp thoái hóa khi một vùng biến mất và mọi giải pháp đều phải tôn trọng điều đó. 

## Phương pháp tiếp cận 

Chiến lược bạo lực trực tiếp chọn mọi cặp hợp lệ`(A, B)`và tính toán ba khu vực. Điều này đúng vì nó đánh giá chính xác hàm mục tiêu, nhưng nó đòi hỏi phải lặp lại một cách đại khái.`(W-1)(H-1)`tiểu bang, trong trường hợp xấu nhất là khoảng một triệu hoạt động. Điều này có thể chấp nhận được dưới những ràng buộc, nhưng nó lãng phí và không sử dụng được cấu trúc của biểu thức. 

Quan sát quan trọng là đối với một vết cắt dọc cố định`A`, cạnh phải trở thành hình chữ nhật có kích thước`(W - A) × H`. Bên trong nó, chúng tôi chọn`B`để chia nó theo chiều ngang. Khu vực bên trái`S1`độc lập với`B`, trong khi`S2`Và`S3`chỉ phụ thuộc vào cách chúng ta phân chia chiều cao. 

Để cố định`A`, vấn đề giảm xuống mức tối đa hóa`min(B, H - B)`bởi vì cả hai`S2`Và`S3`được nhân với cùng một hệ số`(W - A)`. Cách tốt nhất để cực đại hóa cực tiểu của hai hàm tuyến tính đối xứng là chia đều nhất có thể, do đó`B`nên càng gần càng tốt`H/2`càng tốt. Điều này mang lại giá trị tốt nhất có thể của`floor(H/2)`về phía nhỏ hơn. 

Vì vậy đối với mỗi`A`, mức tối thiểu tốt nhất có thể đạt được trong số`S2`Và`S3`trở thành`(W - A) * floor(H/2)`. Bây giờ giá trị thứ ba`S1 = H * A`cạnh tranh với số lượng này, vì vậy chúng tôi giảm vấn đề thành tối ưu hóa một biến duy nhất`A`. 

Tại thời điểm này, bài toán trở thành cực đại một chiều của`min(H*A, C*(W-A))`Ở đâu`C = floor(H/2)`. Hàm này tăng theo`A`ở số hạng thứ nhất và giảm ở số hạng thứ hai, do đó điểm tối ưu nằm gần giao điểm của chúng. Từ`W ≤ 1000`, chúng ta có thể đánh giá một cách an toàn tất cả những gì có thể`A`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Bạo lực đối với A, B | O(HW) | O(1) | Đã chấp nhận | 
| Giảm tối ưu hóa trên A | O(W) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi từng bước chuyển đổi vấn đề thành tối ưu hóa một biến. 

1. Tính toán`C = floor(H / 2)`. 

Điều này thể hiện giá trị tốt nhất có thể của`min(B, H - B)`đối với bất kỳ sự phân chia theo chiều ngang hợp lệ nào. 
2. Cố định vị trí cắt dọc`A`từ`1`ĐẾN`W - 1`. 

Mỗi lựa chọn xác định hình chữ nhật bên trái và chiều rộng của hình chữ nhật bên phải. 
3. Đối với mỗi`A`, tính hai đại lượng cạnh tranh:`S1 = H * A`Và`S23 = C * (W - A)`. 

giá trị`S23`thể hiện mức tối thiểu tốt nhất có thể giữa các hình chữ nhật trên cùng bên phải và dưới cùng bên phải sau khi chọn tối ưu`B`. 
4. Vì điều này`A`, câu trả lời tốt nhất có thể đạt được là`min(S1, S23)`. 

Điều này phản ánh rằng cả ba vùng ít nhất phải lớn như vậy, nên nhỏ nhất trong số đó là hệ số giới hạn. 
5. Theo dõi giá trị lớn nhất của biểu thức này trên tất cả`A`. 

Câu trả lời cuối cùng là điểm cân bằng tốt nhất trong đó vùng bên trái và vùng tối thiểu bên phải tốt nhất có thể càng gần nhau càng tốt. 

### Tại sao nó hoạt động 

Đối với một cố định`A`, vế phải độc lập với vế trái ngoại trừ thông qua hệ số`(W - A)`. Bài toán chia theo chiều ngang bên trong hình chữ nhật bên phải là đối xứng và tuyến tính nên chiến lược tối ưu của nó luôn là chia đều nhất có thể. Điều này làm giảm quyết định hai chiều`(A, B)`vào một tham số duy nhất`A`không làm mất đi tính tối ưu. Vì mọi cấu hình khả thi đều được biểu diễn bởi một số`(A, B)`, và với mỗi`A`chúng tôi chọn tối ưu`B`, không có giải pháp nào tốt hơn có thể tồn tại ngoài mức giảm này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    H, W = map(int, input().split())

    C = H // 2
    best = 0

    for A in range(1, W):
        s1 = H * A
        s23 = C * (W - A)
        best = max(best, min(s1, s23))

    print(best)

if __name__ == "__main__":
    solve()
```Việc thực hiện tuân theo công thức rút gọn trực tiếp. Vòng lặp kết thúc`A`khám phá tất cả các vết cắt dọc hợp lệ. Đối với mỗi người,`s1`là khu vực bên trái và`s23`là nút cổ chai tốt nhất có thể đạt được từ phía bên phải sau khi đặt đường cắt ngang một cách tối ưu. các`min`buộc cả ba vùng ít nhất phải bằng diện tích nhỏ nhất trong số chúng. 

Một lỗi triển khai phổ biến là quên rằng phương pháp tối ưu`B`không cần phải lặp lại một cách rõ ràng. Một người khác đang sử dụng`H/2`thay vì`floor(H/2)`, điều này phá vỡ tính chính xác của số nguyên khi`H`thật kỳ quặc. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
2 2
```Đây`C = 1`. 

| A | S1 = H*A | S23 = C*(W-A) | phút(S1, S23) | 
| --- | --- | --- | --- | 
| 1 | 2 | 1 | 1 | 

Giá trị tốt nhất là`1`. 

Điều này cho thấy trường hợp cực đoan khi cả hai vết cắt đều bị ép ở kích thước tối thiểu và không thể cân bằng ngoài một đơn vị diện tích. 

### Mẫu 2 

đầu vào:```
2 3
```Đây`C = 1`. 

| A | S1 = H*A | S23 = C*(W-A) | phút(S1, S23) | 
| --- | --- | --- | --- | 
| 1 | 2 | 2 | 2 | 
| 2 | 4 | 1 | 1 | 

Tối đa là`2`, đạt được khi`A = 1`. 

Điều này thể hiện sự đánh đổi: tăng`A`cải thiện hình chữ nhật bên trái nhưng thu nhỏ bên phải nhanh hơn mức tăng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(W) | Chúng tôi lặp lại tất cả các lần cắt theo chiều dọc có thể có một lần, mỗi lần trong thời gian không đổi | 
| Không gian | O(1) | Chỉ có một số biến vô hướng được sử dụng | 

Các ràng buộc cho phép lên tới 1000 mỗi chiều, do đó quét tuyến tính trên`W`dễ dàng đủ nhanh ngay cả khi được đánh giá nhiều lần. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import io as _io

    buf = _io.StringIO()
    with redirect_stdout(buf):
        solve()
    return buf.getvalue().strip()

def solve():
    H, W = map(int, input().split())
    C = H // 2
    best = 0
    for A in range(1, W):
        best = max(best, min(H * A, C * (W - A)))
    print(best)

# provided samples
assert run("2 2\n") == "1"
assert run("2 3\n") == "2"

# custom cases
assert run("3 3\n") == "2", "small symmetric case"
assert run("2 1000\n") == "500", "long horizontal strip"
assert run("1000 2\n") == "500", "degenerate width"
assert run("4 4\n") == "4", "balanced square case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 3 | 2 | hành vi phân chia giữa đối xứng | 
| 2 1000 | 500 | sự thống trị của chiều dọc | 
| 1000 2 | 500 | trường hợp cạnh chỉ tồn tại một A | 
| 4 4 | 4 | trường hợp cân bằng trong đó cả hai vết cắt đều căn chỉnh tối ưu | 

## Vỏ cạnh 

Trường hợp một cạnh xảy ra khi`H = 2`. Sau đó`C = 1`, và đường cắt ngang buộc phải chia thành hai dải có chiều cao 1. Thuật toán xử lý việc này một cách chính xác vì`min(B, H-B)`luôn là 1 nên vế phải trở thành`(W - A)`chính xác và việc tối ưu hóa giảm xuống mức cân bằng`2A`Và`W - A`. 

Một trường hợp cạnh khác là khi`W = 2`. Khi đó chỉ có một đường cắt dọc hợp lệ`A = 1`, do đó vòng lặp đánh giá chính xác một cấu hình. Kết quả là`min(H, floor(H/2))`, phù hợp với cấu trúc bắt buộc của hình chữ nhật bên phải. 

Một trường hợp tế nhị cuối cùng là khi`H`thật kỳ quặc. sử dụng`H // 2`đảm bảo rằng việc phân chia không bao giờ đánh giá quá cao mức tối thiểu có thể đạt được. Bất kỳ nỗ lực nào sử dụng phép chia động sẽ tạo ra những so sánh không chính xác do các giả định diện tích không nguyên không hợp lệ.
