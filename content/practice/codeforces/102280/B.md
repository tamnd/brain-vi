---
title: "CF 102280B - \u0421\u0443\u043c\u0430\u0441\u0448\u0435\u0434\u0448\u0438\u0435 \u0433\u043e\u043d\u043a\u0438 \u043d\u0430 \u043c\u0430\u0440\u0448\u0440\u0443\u0442\u043a\u0430\u0445"
description: "Chúng tôi có hai xe buýt nhỏ độc lập. Trong khoảng thời gian một giờ từ 08:00 đến 09:00, mỗi xe buýt nhỏ đến ngã tư đúng một lần và thời gian đến của nó được phân bổ đều trong 3600 giây mỗi giờ. Đèn giao thông bắt đầu có màu xanh vào lúc 0."
date: "2026-08-13T16:04:16+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102280
codeforces_index: "B"
codeforces_contest_name: "2010, \u0422\u0440\u0435\u043d\u0438\u0440\u043e\u0432\u043a\u0430 \u0421\u0413\u0410\u0423 aka \u041a\u043e\u043d\u0442\u0435\u0441\u0442 \u043f\u0440\u043e \u043c\u0430\u0440\u0448\u0440\u0443\u0442\u043a\u0438"
rating: 0
weight: 102280
solve_time_s: 99
verified: true
draft: false
---

[CF 102280B - \u0421\u0443\u043c\u0430\u0441\u0448\u0435\u0434\u0448\u0438\u0435 \u0433\u043e\u043d\u043a\u0438 \u043d\u0430 \u043c\u0430\u0440\u0448\u0440\u0443\u0442\u043a\u0430\u0445](https://codeforces.com/problemset/problem/102280/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 39s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có hai xe buýt nhỏ độc lập. Trong khoảng thời gian một giờ từ 08:00 đến 09:00, mỗi xe buýt nhỏ đến ngã tư đúng một lần và thời gian đến của nó được phân bổ đều trong 3600 giây mỗi giờ. 

Đèn giao thông bắt đầu có màu xanh ở thời điểm 0. Màu xanh kéo dài`g`giây, màu đỏ kéo dài`r`giây và mô hình này lặp lại. Một cuộc đua nguy hiểm bắt đầu chính xác khi cả hai chiếc xe buýt nhỏ đều đến trong lúc đèn đỏ và đặc biệt hơn là cả hai đều đang chờ trong cùng một pha đỏ. Nếu chúng đến trong các giai đoạn màu đỏ khác nhau, chúng sẽ không gặp nhau và không có cuộc đua nào xảy ra. 

Đầu vào chứa`g`Và`r`, cả hai đều nằm trong khoảng từ 1 đến 3600. Đầu ra là xác suất để hai thời điểm đến có cùng khoảng màu đỏ. 

Giới hạn trên của 3600 đủ nhỏ đến mức thậm chí vài nghìn thao tác cũng không đáng kể. Tuy nhiên, giải pháp hữu ích nhất không cần phải mô phỏng từng giây hoặc từng cặp thời gian đến. Cấu trúc tuần hoàn cho phép chúng ta giảm toàn bộ phép tính xác suất thành một số phép tính số học không đổi. Vì lỗi yêu cầu chỉ là`10^-6`, dấu phẩy động có độ chính xác kép thông thường là quá đủ. 

Các trường hợp đặc biệt chính xuất phát từ thực tế là khoảng thời gian quan sát một giờ không nhất thiết phải kết thúc khi kết thúc chu kỳ đèn giao thông. Ví dụ, với`g = 3600`Và`r = 1`, cả giờ đều xanh nên đáp án là chính xác`0`. Một công thức bất cẩn giả định có ít nhất một pha màu đỏ hoàn chỉnh trong một giờ sẽ thêm khoảng màu đỏ một cách không chính xác. 

Một trường hợp ranh giới khác là`g = 3000`,`r = 1000`. Chu kỳ đầu tiên kéo dài 4000 giây. Trong khoảng thời gian quan sát 3600 giây của chúng tôi, đèn xanh lục trong 3000 giây đầu tiên và chỉ đỏ trong 600 giây cuối cùng. Xác suất đúng là`600^2 / 3600^2 = 1 / 36 ≈ 0.0277777778`. 

Một giải pháp luôn thêm một khoảng thời gian màu đỏ hoàn chỉnh`r`cho chu kỳ cuối cùng sẽ sử dụng`1000^2`thay vì`600^2`và đưa ra đáp án sai. 

Đầu vào nhỏ nhất có thể là`g = 1`,`r = 1`. Có 1800 chu kỳ hoàn chỉnh trong một giờ và mỗi khoảng màu đỏ có độ dài 1. Xác suất là`1800 / 3600^2 = 1 / 7200 ≈ 0.000138888889`. 

Trường hợp này rất hữu ích vì nó kiểm tra cả số chu kỳ và thực tế là câu trả lời có thể rất nhỏ. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp có thể chia giờ thành 3600 khoảng thời gian một giây và kiểm tra từng cặp giây đến theo thứ tự. có`3600 * 3600 = 12,960,000`cặp trong trường hợp xấu nhất. Đối với mỗi cặp, chúng ta có thể xác định xem hai giây có thuộc cùng một pha màu đỏ hay không. Điều này đúng về mặt khái niệm vì trạng thái đèn giao thông không đổi trong mỗi khoảng thời gian một giây mở và các ranh giới chính xác có xác suất bằng 0 đối với thời gian đến được phân bổ liên tục. Tuy nhiên, gần 13 triệu lượt kiểm tra cặp là không cần thiết và không phù hợp với giới hạn 0,5 giây ban đầu. 

Ý tưởng vũ lực có hiệu quả vì cuối cùng chúng ta đang đo một diện tích theo bình phương thời gian đến có thể. Hãy để người đến đầu tiên là`x`và thứ hai là`y`. Vì cả hai đều giống nhau trong 3600 giây nên mọi điểm`(x, y)`ở quảng trường`[0, 3600] × [0, 3600]`có mật độ xác suất đồng đều. Một cuộc đua xảy ra khi`x`Và`y`nằm trong cùng một khoảng màu đỏ. 

Quan sát này mang lại sự đơn giản hóa quan trọng. Giả sử một khoảng màu đỏ có độ dài`L`. Cả hai thời gian đến phải nằm trong khoảng thời gian đó một cách độc lập, do đó vùng tương ứng trong`(x, y)`mặt phẳng là hình vuông có cạnh`L`và diện tích`L²`. Các khoảng màu đỏ khác nhau tạo ra các hình vuông rời rạc. Do đó, nếu khoảng màu đỏ trong giờ có độ dài`L1, L2, ...`, tổng diện tích thuận lợi là`L1² + L2² + ...`. 

Tổng diện tích của tất cả các cặp có thể là`3600²`, vậy câu trả lời là`(L1² + L2² + ...) / 3600²`. 

Tất cả các chu kỳ đèn giao thông hoàn chỉnh đều chứa chính xác một khoảng thời gian màu đỏ`r`. Nếu giờ chứa`q`chu kỳ hoàn chỉnh, họ đóng góp`q * r²`. Cũng có thể có một chu kỳ chưa hoàn thành ở cuối. Cho phép`rem`là số giây còn lại sau khi hoàn thành chu kỳ. đầu tiên`g`giây còn lại có màu xanh lá cây. Chỉ một`max(0, rem - g)`giây có thể thuộc về màu đỏ, do đó chu trình không đầy đủ sẽ đóng góp bình phương có độ dài đó. 

Do đó toàn bộ phép tính trở thành thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(3600²) | O(1) | Quá chậm so với giới hạn chặt chẽ | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đặt thời lượng quan sát thành`T = 3600`giây và độ dài chu kỳ đèn giao thông đến`cycle = g + r`. Chia`T`qua`cycle`đưa ra số`q`của các chu kỳ hoàn chỉnh được chứa hoàn toàn trong một giờ. 
2. Tính toán`rem = T % cycle`. Đây là độ dài của chu kỳ chưa hoàn thành vào cuối giờ. Nếu như`rem <= g`, toàn bộ phần còn lại có màu xanh lá cây, vì vậy nó không đóng góp gì vào xác suất cuộc đua. 
3. Nếu`rem > g`, phần sau phần đầu tiên`g`giây của chu kỳ không đầy đủ có màu đỏ. chiều dài của nó là`rem - g`. Đoạn màu đỏ này góp phần`(rem - g)²`đến vùng thuận lợi. 
4. Mỗi`q`chu kỳ hoàn chỉnh chứa một khoảng thời gian màu đỏ`r`. Họ cùng nhau đóng góp`q * r²`. 
5. Cộng các đóng góp đầy đủ và một phần rồi chia cho`T²`. Tử số là diện tích của tất cả các cặp thời gian đến có thể gây ra một cuộc đua, trong khi mẫu số là diện tích của tất cả các cặp thời gian đến có thể xảy ra. 
6. In kết quả giá trị dấu phẩy động với đủ chữ số thỏa mãn yêu cầu`10^-6`khả năng chịu lỗi. 

### Tại sao nó hoạt động 

Với mỗi khoảng màu đỏ có độ dài`L`, một cuộc đua xảy ra chính xác khi cả hai thời điểm đến độc lập đều nằm trong cùng khoảng thời gian đó. Trong không gian hai chiều của các cặp thời gian đến, các khả năng đó tạo thành một bình phương diện tích`L²`. Các hình vuông thuộc các khoảng màu đỏ khác nhau sẽ rời nhau nên diện tích của chúng có thể được cộng lại. Thuật toán tính toán chính xác độ dài của mỗi khoảng màu đỏ giao nhau với cửa sổ một giờ, bao gồm cả khoảng thời gian cuối cùng có thể bị cắt ngắn. Chia chiều dài bình phương của chúng cho tổng diện tích`3600²`do đó đưa ra chính xác xác suất mong muốn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

T = 3600

def solve():
    g, r = map(int, input().split())

    cycle = g + r
    full_cycles = T // cycle
    rem = T % cycle

    favorable = full_cycles * r * r

    partial_red = max(0, rem - g)
    favorable += partial_red * partial_red

    answer = favorable / (T * T)
    print(f"{answer:.12f}")

if __name__ == "__main__":
    solve()
```Ba phép tính số nguyên đầu tiên xác định cách chia giờ thành chu kỳ đèn giao thông đầy đủ và không đầy đủ. Từ`g`Và`r`là số nguyên và nhiều nhất là 3600, không có nguy cơ tràn số nguyên trong Python và số học tương tự cũng an toàn khi triển khai số nguyên 64 bit tiêu chuẩn. 

biểu hiện`max(0, rem - g)`xử lý chu kỳ một phần cuối cùng. Nếu phần còn lại kết thúc trong màu xanh lá cây,`rem - g`là âm, nhưng không có khoảng màu đỏ để đếm. Việc kẹp nó về 0 sẽ tránh việc vô tình thêm một hình vuông dương. 

Phép nhân với`r * r`xảy ra trước khi chuyển đổi sang dấu phẩy động. Điều này giữ cho phép tính hình học chính xác cho đến phép chia cuối cùng. Đáp án cuối cùng được in 12 chữ số sau dấu thập phân, cho độ chính xác cao hơn đáng kể so với yêu cầu`10^-6`. 

Bản thân ranh giới đèn giao thông không cần xử lý đặc biệt. Một thời điểm chính xác duy nhất, chẳng hạn như thời điểm màu xanh lá cây chuyển sang màu đỏ, có xác suất bằng 0 theo phân bố đồng đều liên tục, do đó, cho dù điểm cuối đó được gán cho pha này hay pha kia cũng không thể thay đổi câu trả lời. 

## Ví dụ đã hoạt động 

### Mẫu 1 

cho`g = 1800`Và`r = 1800`, một chu kỳ hoàn chỉnh kéo dài đúng 3600 giây. Toàn bộ giờ bao gồm một khoảng màu xanh lá cây, theo sau là một khoảng màu đỏ. 

| Biến | Giá trị | 
| --- | --- | 
|`T`| 3600 | 
|`cycle`| 3600 | 
|`full_cycles`| 1 | 
|`rem`| 0 | 
|`partial_red`| 0 | 
|`favorable`| 3.240.000 | 
|`answer`| 0,25 | 

Khoảng màu đỏ duy nhất có chiều dài 1800, nên vùng thuận lợi là`1800² = 3,240,000`. Tổng diện tích thời gian đến là`3600² = 12,960,000`, cho`3,240,000 / 12,960,000 = 0.25`. 

### Mẫu 2 

cho`g = 2700`Và`r = 3600`, một chu kỳ hoàn chỉnh kéo dài 6300 giây, dài hơn cả giờ. Không có chu kỳ hoàn chỉnh. Giờ kết thúc sau 3600 giây ở chu kỳ đầu tiên, sau 2700 giây màu xanh lá cây, để lại 900 giây màu đỏ. 

| Biến | Giá trị | 
| --- | --- | 
|`T`| 3600 | 
|`cycle`| 6300 | 
|`full_cycles`| 0 | 
|`rem`| 3600 | 
|`partial_red`| 900 | 
|`favorable`| 810.000 | 
|`answer`| 0,0625 | 

Vùng thuận lợi là`900² = 810,000`. Chia cho`3600²`cho`0.0625`. 

Những dấu vết này chứng minh tại sao chu trình từng phần phải được xử lý riêng biệt. Trong mẫu đầu tiên có đúng một chu trình hoàn chỉnh và không có phần dư. Trong mẫu thứ hai không có chu trình hoàn chỉnh nào cả, tuy nhiên phần cuối cùng của giờ chứa một khoảng màu đỏ đáng kể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ có một số phép tính số học cố định được thực hiện. | 
| Không gian | O(1) | Chỉ có một vài biến số nguyên và dấu phẩy động được lưu trữ. | 

Những ràng buộc cho phép`g`Và`r`lớn tới 3600, nhưng thuật toán không phụ thuộc vào độ lớn của chúng. Nó thực hiện cùng một lượng công việc không đổi cho mọi đầu vào, do đó, nó vừa vặn thoải mái với giới hạn thời gian 0,5 giây và sử dụng bộ nhớ không đáng kể so với giới hạn 64 MB. 

## Trường hợp thử nghiệm```python
import sys
import io

T = 3600

def solve():
    g, r = map(int, input().split())

    cycle = g + r
    full_cycles = T // cycle
    rem = T % cycle

    favorable = full_cycles * r * r

    partial_red = max(0, rem - g)
    favorable += partial_red * partial_red

    answer = favorable / (T * T)
    print(f"{answer:.12f}")

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

def assert_close(inp: str, expected: float, message: str):
    actual = float(run(inp))
    assert abs(actual - expected) <= 1e-12, message

# Provided samples
assert_close("1800 1800\n", 0.25, "sample 1")
assert_close("2700 3600\n", 0.0625, "sample 2")

# Minimum-size input
assert_close("1 1\n", 1 / 7200, "minimum values")

# Maximum-size input
assert_close("3600 3600\n", 0.0, "maximum values")

# Final partial cycle ends exactly at the green/red boundary
assert_close("3600 1\n", 0.0, "no red time inside the hour")

# Only 600 seconds of the final red interval are visible
assert_close("3000 1000\n", 600 * 600 / (3600 * 3600), "partial red interval")

# Many complete cycles, with no partial red interval
assert_close("1000 2000\n", 2000 * 2000 / (3600 * 3600), "complete red interval")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`|`0.000138888889`| Giá trị tối thiểu và nhiều chu kỳ hoàn chỉnh | 
|`3600 3600`|`0.000000000000`| Giá trị tối đa và một giờ chỉ chứa màu xanh lá cây | 
|`3600 1`|`0.000000000000`| Ranh giới nơi quan sát kết thúc chính xác như kết thúc màu xanh lá cây | 
|`3000 1000`|`0.027777777778`| Khoảng màu đỏ hiển thị một phần | 
|`1000 2000`|`0.308641975309`| Hoàn thành các chu kỳ theo sau là phần còn lại chỉ có màu xanh lá cây | 

## Vỏ cạnh 

cho`1 1`, độ dài chu kỳ là 2, vì vậy`3600 // 2 = 1800`chu kỳ hoàn chỉnh và`rem = 0`. Mỗi khoảng màu đỏ có độ dài 1, cho diện tích thuận lợi`1800 * 1² = 1800`. Kết quả là`1800 / 12,960,000 = 0.000138888889`. Thuật toán không bao giờ cần lặp lại 1800 chu kỳ đó. 

Vì`3600 3600`, độ dài chu kỳ là 7200, do đó không có chu kỳ hoàn chỉnh nào và`rem = 3600`. Vì phần còn lại chính xác là khoảng thời gian xanh,`partial_red = max(0, 3600 - 3600) = 0`. Câu trả lời là không. Điều này phát hiện việc triển khai vô tình coi điểm cuối của màu xanh lá cây là khoảng màu đỏ có độ dài dương. 

Vì`3600 1`, độ dài chu kỳ là 3601. Một lần nữa, không có chu kỳ hoàn chỉnh nào và quá trình quan sát 3600 giây kết thúc đúng một giây trước khi pha đỏ đầu tiên bắt đầu. Chiều dài một phần màu đỏ là`max(0, 3600 - 3600) = 0`, vì vậy câu trả lời là không. 

Vì`3000 1000`, độ dài chu kỳ là 4000. Giờ chứa 3000 giây đầu tiên màu xanh lá cây và sau đó là 600 giây màu đỏ. Như vậy`full_cycles = 0`,`rem = 3600`, Và`partial_red = 600`. Vùng thuận lợi là`600² = 360,000`, sản xuất`360,000 / 12,960,000 = 0.027777777778`. Đây là trường hợp kiểu riêng biệt chính vì khoảng màu đỏ hiển thị ngắn hơn khoảng thời gian màu đỏ được định cấu hình. 

Vì`1000 2000`, độ dài chu kỳ là 3000. Giờ chứa một chu kỳ hoàn chỉnh và thời gian còn lại là 600 giây. Phần còn lại vẫn nằm trong giai đoạn xanh tiếp theo nên không đóng góp gì. Vùng thuận lợi duy nhất đến từ khoảng màu đỏ hoàn chỉnh có chiều dài 2000, mang lại`2000² / 3600² = 0.308641975309`. Điều này xác minh rằng phần còn lại không nên được tính chỉ vì nó tồn tại, chỉ phần của nó sau giai đoạn xanh mới quan trọng.
