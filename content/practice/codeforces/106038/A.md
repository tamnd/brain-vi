---
title: "CF 106038A - Salvador"
description: "Chúng tôi đang theo dõi cuộc bỏ phiếu giữa bốn ứng cử viên cố định là rùa Rafael, Leonardo, Donatello và Michelangelo. Mỗi ứng cử viên đã có một số phiếu bầu nhất định và còn một lượng phiếu bầu còn lại chưa được bầu."
date: "2026-06-20T13:31:01+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106038
codeforces_index: "A"
codeforces_contest_name: "UNICAMP Selection Contest 2025"
rating: 0
weight: 106038
solve_time_s: 45
verified: true
draft: false
---

[CF 106038A - Salvador](https://codeforces.com/problemset/problem/106038/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 45s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang theo dõi cuộc bỏ phiếu giữa bốn ứng cử viên cố định là rùa Rafael, Leonardo, Donatello và Michelangelo. Mỗi ứng cử viên đã có một số phiếu bầu nhất định và còn một lượng phiếu bầu còn lại chưa được bầu. Mỗi phiếu bầu còn lại sẽ thuộc về chính xác một trong bốn ứng cử viên, nhưng chúng tôi không biết chúng sẽ được phân bổ như thế nào. 

Một ứng cử viên chỉ thắng nếu sau khi tất cả số phiếu còn lại được phân bổ theo cách có lợi nhất cho ứng cử viên đó, họ giành được hơn một nửa tổng số phiếu bầu cuối cùng. Nhiệm vụ là xác định những con rùa nào vẫn có con đường lý thuyết để trở thành người chiến thắng đa số duy nhất. 

Kích thước đầu vào có cấu trúc không đổi vì luôn có bốn ứng cử viên. Mức độ khác nhau duy nhất là số phiếu bầu còn lại, có thể lớn. Điều này ngay lập tức loại trừ mọi nhu cầu về cấu trúc dữ liệu phức tạp hoặc mô phỏng phiếu bầu. Bất kỳ giải pháp nào cố gắng liệt kê sự phân bổ số phiếu còn lại đều có tính chất theo cấp số nhân và không thể thực hiện được ngay cả đối với các giá trị vừa phải, vì mỗi phiếu còn lại sẽ chọn một trong bốn ứng cử viên một cách độc lập. 

Một trường hợp khó nhận thấy xảy ra khi không có ứng cử viên nào hiện dẫn đầu rõ ràng nhưng số phiếu còn lại không đủ để tạo đa số cho bất kỳ ai. Ví dụ: nếu tất cả phiếu bầu đã được bỏ và không có ai vượt quá một nửa thì không có người chiến thắng. Một trường hợp góc cạnh khác là khi một ứng cử viên đã dẫn trước mạnh mẽ nhưng vẫn có thể bị vượt qua nếu số phiếu còn lại có thể được phân phối lại một cách tối ưu. 

Hãy xem xét kịch bản này: số phiếu hiện tại là 10 0 0 0 với 5 phiếu còn lại. Tổng số là 15 nên đa số cần ít nhất 8 phiếu. Rafael đã có 10 nên anh ấy thỏa mãn điều kiện một cách tầm thường, nhưng chúng ta vẫn phải kiểm tra những người khác một cách cẩn thận. Leonardo, Donatello và Michelangelo sẽ cần tất cả 5 phiếu còn lại cộng với việc bù đắp thâm hụt bổ sung, điều này là không thể. 

Một kịch bản khác: 1 1 1 1 với 4 phiếu còn lại. Tổng số là 8, ngưỡng đa số là 5. Mỗi ứng cử viên có thể nhận được tất cả 4 phiếu còn lại, kết thúc ở mức 5, vì vậy tất cả đều có khả năng chiến thắng. Cách giải thích ngây thơ cho rằng chỉ những nhà lãnh đạo hiện tại mới quan trọng ở đây sẽ không chính xác. 

## Phương pháp tiếp cận 

Ý tưởng bạo lực sẽ là phân phối từng phiếu bầu còn lại cho bốn ứng cử viên và mô phỏng tất cả các kết quả có thể xảy ra. Mỗi phiếu bầu có 4 lựa chọn, vì vậy điều này tạo ra$4^r$khả năng ở đâu$r$là số phiếu còn lại. Ngay cả đối với$r = 20$, điều này đã vượt quá một tỷ cấu hình, khiến nó không thể thực hiện được. 

Quan sát quan trọng là chúng ta không cần mô phỏng phân phối. Đối với bất kỳ ứng cử viên cố định nào, chúng tôi chỉ cần biết liệu có tồn tại sự phân bổ số phiếu còn lại cho phép họ vượt quá một nửa tổng số cuối cùng hay không. Kịch bản tốt nhất có thể xảy ra đối với một ứng cử viên là khi tất cả số phiếu còn lại thuộc về họ, vì việc trao phiếu cho người khác chỉ khiến nhiệm vụ của họ trở nên khó khăn hơn. Điều này làm giảm vấn đề xuống còn việc kiểm tra tính khả thi trực tiếp cho mỗi ứng viên. 

Chúng tôi tính tổng số phiếu bầu cuối cùng là tổng của tất cả các phiếu bầu hiện tại cộng với số phiếu bầu còn lại. Ngưỡng chiến thắng đúng là hơn một nửa tổng số này. Đối với mỗi ứng cử viên, chúng tôi kiểm tra xem số phiếu hiện tại của họ cộng với tất cả số phiếu còn lại có lớn hơn ngưỡng này hay không. Nếu có thì họ vẫn có khả năng chiến thắng; nếu không, họ sẽ bị loại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(4^R) | O(1) | Quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta quy vấn đề về việc đánh giá một bất đẳng thức đơn giản cho mỗi ứng viên. 

1. Đọc bốn phiếu bầu hiện tại và số phiếu bầu còn lại. Chúng tôi cũng tính toán tổng số phiếu bầu sau khi tất cả các cuộc bỏ phiếu kết thúc bằng cách cộng bốn giá trị hiện tại và cộng số phiếu bầu còn lại. 
2. Đối với mỗi ứng cử viên, hãy tính điểm cuối cùng cao nhất có thể bằng cách giả sử họ nhận được tất cả phiếu bầu còn lại. Đây là trường hợp có lợi nhất cho ứng viên đó và đưa ra giới hạn trên về kết quả có thể đạt được của họ. 
3. Tính ngưỡng đa số bằng một nửa tổng số phiếu bầu cuối cùng và yêu cầu phải lớn hơn giá trị này. Sự nghiêm khắc này rất quan trọng vì hòa giải không tạo ra người chiến thắng. 
4. Đối với mỗi ứng viên, hãy kiểm tra xem điểm tốt nhất của họ có vượt quá ngưỡng đa số hay không. Nếu đúng như vậy, chúng tôi đánh dấu họ vẫn còn cơ hội chiến thắng. 
5. Xuất tất cả các ứng cử viên như vậy theo thứ tự bảng chữ cái. Nếu không có người nào đủ điều kiện, xuất ra chuỗi cho biết không có người chiến thắng. 

### Tại sao nó hoạt động 

Đối với bất kỳ ứng cử viên nào, việc phân phối số phiếu bầu còn lại ra khỏi họ chỉ có thể làm giảm tổng số phiếu bầu cuối cùng của họ, trong khi tổng số phiếu bầu vẫn cố định bất kể phân phối như thế nào. Vì điều kiện chiến thắng chỉ phụ thuộc vào tổng số cuối cùng tương ứng với một ngưỡng cố định, nên kết quả tốt nhất có thể đạt được đối với một ứng cử viên là bằng cách trao cho họ mọi phiếu bầu còn lại. Nếu ngay cả kịch bản tối đa này cũng không vượt qua được ngưỡng đa số thì không có cách phân bổ nào khác có thể giúp họ, bởi vì bất kỳ sự phân bổ thay thế nào cũng sẽ làm giảm nghiêm trọng điểm của họ mà không thay đổi ngưỡng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    r, l, d, m, rem = map(int, input().split())
    names = [("Donatello", d), ("Leonardo", l), ("Michelangelo", m), ("Rafael", r)]

    total = r + l + d + m + rem
    threshold = total // 2  # must be strictly greater

    res = []

    for name, cur in names:
        if cur + rem > threshold:
            res.append(name)

    if not res:
        print("sem vencedores")
    else:
        res.sort()
        for x in res:
            print(x)

if __name__ == "__main__":
    main()
```Giải pháp bắt đầu bằng cách đọc năm số nguyên và nhóm các ứng cử viên với phiếu bầu hiện tại của họ. Tổng số phiếu bầu cuối cùng được tính một lần vì nó không phụ thuộc vào cách phân bổ số phiếu còn lại. Ngưỡng này được tính bằng phép chia số nguyên và chúng tôi dựa vào bất đẳng thức nghiêm ngặt khi kiểm tra tính khả thi. 

Mỗi ứng cử viên được đánh giá độc lập bằng cách cộng tất cả số phiếu còn lại vào số phiếu hiện tại của họ. Điều này trực tiếp thực hiện lý luận trường hợp tốt nhất. Việc sắp xếp đảm bảo đầu ra theo thứ tự bảng chữ cái, vì thứ tự ứng cử viên trong đầu vào không được đảm bảo khớp với thứ tự đầu ra được yêu cầu. 

Một cạm bẫy phổ biến là sử dụng`>=`thay vì`>`. Bài toán đòi hỏi phải có hơn một nửa, vì vậy sự bình đẳng không được coi là chiến thắng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
0 0 0 0 10000
```| Ứng viên | Hiện tại | Còn lại | Trường hợp tốt nhất | Tổng Số Phiếu | Ngưỡng | Thắng? | 
| --- | --- | --- | --- | --- | --- | --- | 
| Rafael | 0 | 10000 | 10000 | 10000 | 5000 | Có | 
| Leonardo | 0 | 10000 | 10000 | 10000 | 5000 | Có | 
| Donatello | 0 | 10000 | 10000 | 10000 | 5000 | Có | 
| Michelangelo | 0 | 10000 | 10000 | 10000 | 5000 | Có | 

Tất cả các ứng cử viên đều vượt quá một nửa trong trường hợp tốt nhất của họ, vì vậy tất cả vẫn là người chiến thắng khả thi. 

### Ví dụ 2 

đầu vào:```
5 4 3 3 0
```| Ứng viên | Hiện tại | Còn lại | Trường hợp tốt nhất | Tổng Số Phiếu | Ngưỡng | Thắng? | 
| --- | --- | --- | --- | --- | --- | --- | 
| Rafael | 5 | 0 | 5 | 15 | 7 | Không | 
| Leonardo | 4 | 0 | 4 | 15 | 7 | Không | 
| Donatello | 3 | 0 | 3 | 15 | 7 | Không | 
| Michelangelo | 3 | 0 | 3 | 15 | 7 | Không | 

Không ứng cử viên nào có thể đạt được đa số vì không còn phiếu bầu nào và tất cả đều ở dưới ngưỡng. 

Những dấu vết này cho thấy thuật toán đánh giá các ứng viên một cách nhất quán theo ngưỡng toàn cầu cố định trong khi tối đa hóa sự đóng góp của từng cá nhân họ một cách độc lập. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ có bốn ứng viên được kiểm tra bằng các phép tính số học theo thời gian không đổi | 
| Không gian | O(1) | Chỉ có một lượng lưu trữ cố định cho bộ đếm và danh sách đầu ra | 

Việc tính toán không phụ thuộc vào độ lớn của các giá trị đầu vào, do đó nó phù hợp với mọi ràng buộc. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from math import isclose

    r, l, d, m, rem = map(int, _sys.stdin.readline().split())
    names = [("Donatello", d), ("Leonardo", l), ("Michelangelo", m), ("Rafael", r)]
    total = r + l + d + m + rem
    threshold = total // 2

    res = [name for name, cur in names if cur + rem > threshold]

    if not res:
        return "sem vencedores"
    res.sort()
    return "\n".join(res)

# provided samples
assert run("0 0 0 0 10000") == "Donatello\nLeonardo\nMichelangelo\nRafael"
assert run("5 4 3 3 0") == "sem vencedores"

# custom cases
assert run("1 1 1 1 4") == "Donatello\nLeonardo\nMichelangelo\nRafael"
assert run("10 0 0 0 5") == "Rafael"
assert run("3 3 3 3 1") == "sem vencedores"
assert run("1000000 0 0 0 0") == "Rafael"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 1 1 4 | tất cả tên | cà vạt đối xứng với sự phân phối lại đầy đủ | 
| 10 0 0 0 5 | Rafael | nhà lãnh đạo mạnh mẽ đã vượt ngưỡng | 
| 3 3 3 3 1 | bán hàng | không đủ phiếu bầu duy nhất để tạo đa số | 
| 1000000 0 0 0 0 | Rafael | không còn phiếu bầu, kiểm tra đa số trực tiếp | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi không còn phiếu bầu nào. Ví dụ, đầu vào`5 4 3 3 0`dẫn đến một kết quả cố định và không có khả năng thay đổi điểm số. Thuật toán tính tổng điểm là 15 và ngưỡng là 7. Vì tất cả điểm hiện tại đều thấp hơn hoặc bằng ngưỡng này nên không có ứng viên nào vượt qua được điều kiện nghiêm ngặt. Việc kiểm tra đánh giá chính xác từng ứng viên như`cur + 0 > threshold`, không tạo ra người chiến thắng. 

Một trường hợp khác là khi tất cả các ứng cử viên đều ngang nhau và có đủ số phiếu bầu còn lại để tạo nên đa số. Vì`1 1 1 1 4`, tổng trở thành 8 và ngưỡng là 4. Mỗi ứng viên có trường hợp tốt nhất là 5, vượt quá ngưỡng nên tất cả đều được in. Thuật toán xử lý việc này một cách tự nhiên vì nó đánh giá từng ứng viên một cách độc lập mà không giả định vấn đề xếp hạng hiện tại.
