---
title: "CF 102191F - Tính tổng rồi nhân"
description: "Chúng ta cần cắt mảng thành các mảng con liên tiếp. Mỗi mảng con đóng góp tổng phần tử của nó dưới dạng một thừa số và mục tiêu là tối đa hóa tích của tất cả các thừa số đó. Đầu ra là mảng ban đầu có/chèn vào giữa các phần liên tiếp."
date: "2026-08-24T15:02:12+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102191
codeforces_index: "F"
codeforces_contest_name: "PSUT Coding Marathon 2019"
rating: 0
weight: 102191
solve_time_s: 1481
verified: true
draft: false
---

[CF 102191F - Tính tổng rồi nhân](https://codeforces.com/problemset/problem/102191/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 24m 41s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta cần cắt mảng thành các mảng con liên tiếp. Mỗi mảng con đóng góp tổng phần tử của nó dưới dạng một thừa số và mục tiêu là tối đa hóa tích của tất cả các thừa số đó. Đầu ra là mảng ban đầu với`/`chèn vào giữa các phần liên tiếp. 

Khó khăn chính là tích có thể trở nên lớn về mặt thiên văn, vì vậy nhiệm vụ không yêu cầu chúng ta tính tích cực đại. Chúng ta chỉ cần khôi phục một phân vùng đạt được nó. Các ràng buộc cho phép tối đa (3\cdot10^5) phần tử, do đó giải pháp (O(n)) hoặc (O(n\log n)) là phù hợp. Bất cứ điều gì theo cấp số nhân đều là không thể ngay lập tức và ngay cả một chương trình động (O(n^2)) cũng sẽ yêu cầu khoảng (9\cdot10^{10}) lần lặp trong trường hợp xấu nhất. 

Thực tế là mọi giá trị mảng đều dương là nguyên nhân khiến bài toán trở thành một cấu trúc tham lam đơn giản. Việc triển khai bất cẩn có thể thất bại khi một phần có tổng (1). Ví dụ, đối với```
2
3 1
```đầu ra đúng là```
3 1
```bởi vì phân vùng`[3] / [1]`có sản phẩm (3), trong khi`[3 1]`có sản phẩm (4). Chiến lược cắt bất cứ khi nào tổng hiện tại đạt ít nhất (2), sau đó in một cách mù quáng singleton cuối cùng, sẽ tạo ra phân vùng dưới mức tối ưu không hợp lệ`[3] / [1]`. 

Một trường hợp ranh giới khác là một mảng bao gồm toàn bộ mảng. Vì```
3
1 1 1
```đầu ra đúng là```
1 1 1
```còn hơn là`[1 1] / [1]`. Phần sau có tích (2), trong khi phần đơn có tổng (3) và tích (3). Phần còn sót lại cuối cùng phải được hợp nhất với phần trước. 

Mảng một phần tử cũng cần được xem xét rõ ràng. Vì```
1
1
```không có phần lân cận nào có thể hợp nhất phần tử này, vì vậy câu trả lời khả thi duy nhất chỉ đơn giản là`1`. 

## Phương pháp tiếp cận 

Một giải pháp brute-force có thể liệt kê mọi tập hợp cắt giảm có thể. Có (n-1) khoảng trống giữa các phần tử liền kề và mỗi khoảng trống có thể chứa một phần bị cắt hoặc không, tạo ra các phân vùng chính xác (2^{n-1}). Đối với mỗi phân vùng, chúng ta có thể tính toán tất cả các tổng các phần và tích của chúng, sau đó giữ lại phân vùng tốt nhất. Ngay cả khi sản phẩm được bảo trì tăng dần, điều này vẫn yêu cầu đánh giá phân vùng (2^{n-1}). Nếu mọi phân vùng được đánh giá bằng cách quét toàn bộ mảng thì chi phí sẽ trở thành (O(n2^n)). Tại (n=3\cdot10^5), thậm chí bản thân (2^{n-1}) đã vượt quá mọi giới hạn thực tế. 

Quan sát loại bỏ tìm kiếm theo cấp số nhân là bất đẳng thức 

[ 
xy \ge x+y 
] 

bất cứ khi nào (x,y\ge2). Xét hai phần lân cận có tổng là (x) và (y). Giữ vết cắt sẽ cho sản phẩm (xy), trong khi loại bỏ vết cắt sẽ cho (x+y). Nếu cả hai tổng ít nhất là (2), việc giữ nguyên mức cắt không bao giờ làm cho câu trả lời tệ hơn. 

Điều này có nghĩa là một giải pháp tối ưu sẽ tạo ra càng nhiều phần có tổng ít nhất (2) càng tốt. Phần nguy hiểm duy nhất là phần có tổng (1). Vì mọi giá trị mảng đều dương nên phần có tổng (1) bao gồm chính xác một phần tử mảng bằng (1). Phần như vậy nên được hợp nhất với phần liền kề bất cứ khi nào phần liền kề tồn tại, bởi vì thay thế hệ số (1) và hệ số (x) bằng (x+1) sẽ làm tăng nghiêm ngặt tích số. 

Câu hỏi còn lại là làm thế nào để tối đa hóa số phần có tổng ít nhất là (2). Vị trí cắt sớm nhất có thể là vị trí đầu tiên mà phần hiện tại đạt đến tổng (2). Việc cắt ở đó luôn an toàn và để lại hậu tố lớn nhất có thể cho các phần tiếp theo. Việc lặp lại điều này một cách tham lam sẽ mang lại số lượng bộ phận hoàn chỉnh tối đa có thể. Nếu quá trình quét kết thúc với một dữ liệu chưa được sử dụng`1`, phần tử cuối cùng đó không thể tự mình tạo thành phần có lợi nhuận hợp lệ nên được sáp nhập vào phần trước. 

Phương pháp brute-force hoạt động vì nó xem xét rõ ràng mọi vị trí có thể cắt, nhưng không thành công vì có nhiều vị trí như vậy theo cấp số nhân. Việc quan sát về (xy\ge x+y) thay đổi mục tiêu từ tìm kiếm trên các sản phẩm sang tối đa hóa số lượng phần hợp lệ, có thể được thực hiện bằng một lần quét từ trái sang phải. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n2^n)) | (O(n)) | Quá chậm | 
| Tối ưu | (O(n)) | (O(n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Bắt đầu ở phần tử mảng đầu tiên và duy trì phần bắt đầu của phần hiện tại cùng với tổng chạy của nó. Phần hiện tại không bị cắt cho đến khi tổng của nó đạt ít nhất (2), vì phần có tổng (1) không bao giờ hữu ích khi có phần tử khác tồn tại. 
2. Quét mảng từ trái sang phải. Bất cứ khi nào tổng chạy ít nhất là (2), hãy ghi lại phần cắt ngay sau phần tử hiện tại và bắt đầu phần mới. Cắt ở vị trí sớm nhất có thể sẽ tối đa hóa số lượng bộ phận hoàn chỉnh vẫn có thể được tạo sau này. 
3. Sau khi quét, kiểm tra xem một số hậu tố có còn nguyên không. Bởi vì mọi lần cắt trước đó đều được thực hiện ngay khi đạt đến tổng (2), hậu tố chưa hoàn thành chỉ có thể có tổng (1). Vì tất cả các giá trị đều dương nên hậu tố đó chính xác là một phần tử cuối cùng bằng`1`. 
4. Nếu có một trận chung kết như vậy`1`và ít nhất một phần trước tồn tại, hãy loại bỏ phần cắt trước đó và mở rộng phần trước đó thông qua phần tử cuối cùng. Điều này loại bỏ phần tổng-(1) và việc hợp nhất nó sẽ làm tăng tích. 
5. Nếu toàn bộ mảng bao gồm một phần tử cuối cùng đó thì không thể hợp nhất được. Phân vùng một phần tử là phân vùng duy nhất có thể. 
6. In mọi mảng con kết quả, đặt`/`giữa các phần liên tiếp. 

### Tại sao nó hoạt động 

Giả sử hai phần liền kề có tổng (x,y\ge2). Đóng góp của chúng khi được tách riêng là (xy), khi hợp nhất chúng sẽ cho (x+y). Kể từ khi 

[ 
xy-x-y=(x-1)(y-1)-1\ge0, 
] 

tách chúng ra không bao giờ làm giảm sản phẩm. Do đó, khi mỗi phần có tổng ít nhất (2) thì việc tối đa hóa số phần là đủ. 

Quá trình quét tham lam luôn chọn điểm cuối sớm nhất có thể cho mọi phần. Trước khi đạt đến điểm cuối tham lam, tổng tích lũy sẽ thấp hơn (2), do đó không có phần hợp lệ nào có thể kết thúc sớm hơn. Do đó, lần cắt tham lam đầu tiên không thể xảy ra muộn hơn lần cắt đầu tiên của bất kỳ phân vùng nào thành các phần của tổng ít nhất (2). Áp dụng cùng một đối số cho hậu tố còn lại sẽ chứng minh cùng một thuộc tính cho mỗi lần cắt tiếp theo. 

Nếu quá trình quét tham lam kết thúc bằng kết quả cuối cùng`1`, phần tử đó không thể tạo thành một phần hợp lệ riêng biệt. Bất kỳ phân vùng nào có thêm một phần sẽ phải để lại một hậu tố sau phần cắt tương ứng trước đó, nhưng hậu tố đó sẽ chỉ chứa phần này`1`, điều này là không thể đối với một phân vùng có tất cả các phần có tổng ít nhất là (2). Hợp nhất trận chung kết`1`với phần trước là tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve(data: str) -> str:
    it = iter(data.split())
    n = int(next(it))
    a = [int(next(it)) for _ in range(n)]

    cuts = []
    start = 0
    current_sum = 0

    for i, x in enumerate(a):
        current_sum += x

        if current_sum >= 2:
            cuts.append(i)
            start = i + 1
            current_sum = 0

    # If one element remains, it must be a single 1.
    # Merge it into the previous part.
    if start < n:
        if cuts:
            cuts.pop()
            cuts.append(n - 1)
        else:
            cuts.append(n - 1)

    parts = []
    start = 0

    for end in cuts:
        parts.append(" ".join(map(str, a[start:end + 1])))
        start = end + 1

    return " / ".join(parts)

def main():
    data = sys.stdin.read()
    sys.stdout.write(solve(data) + "\n")

if __name__ == "__main__":
    main()
```Đầu vào được đọc với`sys.stdin.read()`vì chỉ có một trường hợp thử nghiệm và toàn bộ dữ liệu đầu vào đủ nhỏ để lưu trong bộ nhớ. Điều này cũng tránh được chi phí phân tích cú pháp lặp đi lặp lại trong một vấn đề với mảng phần tử (3\cdot10^5). 

các`current_sum`biến đại diện cho tổng của phần hiện đang được xây dựng. Ngay khi đạt tới (2), chỉ số hiện tại sẽ trở thành điểm cuối bị cắt. Bởi vì tất cả các giá trị đều dương nên khi tổng đạt đến (2), việc mở rộng phần này hơn nữa không thể giúp chúng ta tạo ra nhiều phần hơn. 

các`cuts`list lưu trữ điểm cuối bao gồm của mọi phần đã hoàn thành. Của`start < n`sau khi quét, một phần tử còn sót lại. Giá trị của nó phải là`1`. Khi một phần khác tồn tại, phần cắt cuối cùng sẽ bị loại bỏ và thay thế bằng`n - 1`, hợp nhất phần tử còn sót lại vào phần trước. 

Trường hợp không có phần cắt trước chỉ xảy ra khi toàn bộ mảng là một phần tử cuối cùng, cụ thể là`n = 1`Và`a[0] = 1`. Trong trường hợp đó, phần tử đơn lẻ đã là phân vùng duy nhất có thể. 

Số nguyên Python không bị tràn, mặc dù giải pháp này không bao giờ tính toán tích số. Điều đó rất hữu ích ở đây vì sản phẩm tối ưu thực tế có thể có hàng triệu chữ số thập phân. Việc triển khai chỉ thực hiện các khoản tiền được giới hạn bởi kích thước và giá trị đầu vào, cộng với các thao tác chỉ mục. 

Đầu ra được xây dựng từ các chuỗi phần hoàn chỉnh và được nối bằng cách sử dụng`" / "`, cung cấp chính xác một khoảng trắng ở cả hai bên của mỗi dấu gạch chéo theo yêu cầu. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đối với đầu vào```
4
8 1 1 3
```quá trình quét hoạt động như sau. 

| Chỉ mục | Giá trị | Tổng chạy | Hành động | Cắt | 
| --- | --- | --- | --- | --- | 
| 0 | 8 | 8 | Cắt sau 8 |`[0]`| 
| 1 | 1 | 1 | Tiếp tục |`[0]`| 
| 2 | 1 | 2 | Cắt sau giây 1 |`[0, 2]`| 
| 3 | 3 | 3 | Cắt sau 3 |`[0, 2, 3]`| 

Mỗi bộ phận được sản xuất có tổng ít nhất là (2). Phân vùng kết quả là```
8 / 1 1 / 3
```với các giá trị hệ số (8,2,3), cho kết quả (48). Dấu vết thể hiện quy tắc tham lam chính: cắt ngay lập tức khi tổng hiện tại lần đầu tiên đạt đến (2). 

### Mẫu 2 

cho```
3
1 1 1
```quá trình quét là: 

| Chỉ mục | Giá trị | Tổng chạy | Hành động | Cắt | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 1 | Tiếp tục |`[]`| 
| 1 | 1 | 2 | Cắt sau giây 1 |`[1]`| 
| 2 | 1 | 1 | Để lại dang dở |`[1]`| 
| Kết thúc | | 1 còn sót lại | Hợp nhất với phần trước |`[2]`| 

Đầu ra cuối cùng là```
1 1 1
```Phần quan trọng của dấu vết này là sự hợp nhất cuối cùng. Quá trình quét tham lam tạo ra một phần hoàn chỉnh`[1,1]`, nhưng phần còn lại`1`không thể là một yếu tố hữu ích riêng biệt. Việc hợp nhất nó tạo ra một thừa số duy nhất (3), tốt hơn tích (2\cdot1). 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n)) | Mảng được quét một lần và mỗi phần tử được xử lý với số lần không đổi. | 
| Không gian | (O(n)) | Mảng, vị trí cắt và các phần đầu ra yêu cầu bộ nhớ tuyến tính. | 

Với (n\le3\cdot10^5), việc quét tuyến tính có thể dễ dàng nằm trong mức độ phức tạp dự định. Thuật toán không bao giờ tự xây dựng hoặc so sánh các sản phẩm khổng lồ, điều này giúp duy trì cả thời gian chạy và bộ nhớ thực tế dưới giới hạn 1 giây và 256 MB. 

## Trường hợp thử nghiệm```
# helper: run solution on input string, return output string
import io

def run(inp: str) -> str:
    return solve(inp).strip()

# Provided samples
assert run("4\n8 1 1 3\n") == "8 / 1 1 / 3", "sample 1"
assert run("3\n1 1 1\n") == "1 1 1", "sample 2"

# Minimum-size input
assert run("1\n1\n") == "1", "single element equal to 1"

# All equal values
assert run("4\n2 2 2 2\n") == "2 / 2 / 2 / 2", "all equal values"

# Boundary case with a final singleton 1
assert run("3\n3 1 1\n") == "3 / 1 1", "trailing singleton handling"

# Maximum-size input, all ones
n = 300000
inp = str(n) + "\n" + ("1 " * (n - 1)) + "1\n"
expected = " / ".join(["1 1"] * (n // 2))
assert run(inp) == expected, "maximum-size input"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1`|`1`| Mảng có kích thước tối thiểu và trường hợp không thể hợp nhất | 
|`4 / 2 2 2 2`|`2 / 2 / 2 / 2`| Chia các phần có tổng ít nhất (2) không bao giờ gây hại | 
|`3 / 3 1 1`|`3 / 1 1`| Xử lý đúng một singleton cuối cùng và một giá trị lớn ban đầu | 
|`300000 / 1 1 ... 1`| 150000 phần của`1 1`| Tối đa (n), hiệu suất tuyến tính và các quyết định ranh giới lặp đi lặp lại | 

## Vỏ cạnh 

Đối với đầu vào tối thiểu`1 / 1`, quá trình quét không bao giờ đạt đến tổng (2), do đó không có vết cắt nào được hoàn thành. Hậu tố còn lại là toàn bộ mảng nhưng không có phần nào trước đó để hợp nhất nó vào. Do đó, thuật toán in`1`, đây là phân vùng duy nhất có thể. 

Vì`3 / 3 1`, phần tử đầu tiên ngay lập tức tạo thành một phần hoàn chỉnh có tổng (3). trận chung kết`1`vẫn còn sau khi quét. Vì có phần trước nên thuật toán loại bỏ phần cắt sau`3`và hợp nhất phần tử cuối cùng, tạo ra`3 1`. Sản phẩm là (4), tốt hơn sản phẩm riêng lẻ (3). 

Vì`3 / 1 1 1`, hai số đầu đạt tổng (2) nên thuật toán tạm thời tạo`[1,1]`. cuối cùng`1`vẫn còn là một singleton. Việc hợp nhất cuối cùng thay đổi phân vùng thành`[1,1,1]`, có hệ số là (3). Điều này là tối ưu vì tích thay thế (2\cdot1) nhỏ hơn. 

Đối với một mảng có ít nhất tất cả các giá trị (2), mọi phần tử có thể ngay lập tức tạo thành phần riêng của nó. Ví dụ,`4 / 2 2 2 2`trở thành`2 / 2 / 2 / 2`. Mọi cặp thừa số lân cận đều thỏa mãn (xy\ge x+y), do đó việc kết hợp hai phần bất kỳ không thể cải thiện sản phẩm. Do đó, phân vùng tham lam có số phần tối đa có thể và là tối ưu. 

Đối với đầu vào có kích thước tối đa với (300000), mỗi cặp đều đạt tổng (2), do đó quá trình quét tạo ra 150000 phần và kết thúc mà không có phần thừa. Thuật toán thực hiện chính xác một quyết định theo thời gian không đổi cho mỗi phần tử mảng và không cần bất kỳ phép tính tích nào, làm cho đầu vào lớn hoạt động giống như ví dụ tất cả những cái nhỏ.
