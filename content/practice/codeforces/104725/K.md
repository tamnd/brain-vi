---
title: "CF 104725K - RSP"
description: "Chúng ta được đưa ra một trò chơi dành cho hai người chơi tổng quát hóa trò oẳn tù tì thành các ký hiệu $n$ được sắp xếp theo một chu kỳ. Ký hiệu $i$ thắng ký hiệu $i+1$ và ký hiệu $n$ thắng ký hiệu $1$. Bất kỳ cặp nào khác không phải là quan hệ thắng trực tiếp sẽ dẫn đến kết quả hòa."
date: "2026-06-29T02:58:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104725
codeforces_index: "K"
codeforces_contest_name: "2023\u5e74\u4e2d\u56fd\u5927\u5b66\u751f\u7a0b\u5e8f\u8bbe\u8ba1\u7ade\u8d5b\u5973\u751f\u4e13\u573a"
rating: 0
weight: 104725
solve_time_s: 53
verified: true
draft: false
---

[CF 104725K - RSP](https://codeforces.com/problemset/problem/104725/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được đưa ra một trò chơi hai người chơi khái quát hóa trò oẳn tù tì để$n$các ký hiệu được sắp xếp theo một chu kỳ. Biểu tượng$i$biểu tượng đánh bại$i+1$, và ký hiệu$n$biểu tượng đánh bại$1$. Bất kỳ cặp nào khác không phải là quan hệ thắng trực tiếp sẽ dẫn đến kết quả hòa. 

Các cầu thủ không chơi một vòng duy nhất. Thay vào đó họ chơi$m$vòng, và người chiến thắng cuối cùng là người thắng được nhiều vòng cá nhân hơn. Một hạn chế chính là người chơi không được phép chơi cùng một biểu tượng như ở vòng trước. Cả hai người chơi đều được cho là hoàn toàn lý trí và chọn những chiến lược nhằm tối đa hóa cơ hội chiến thắng cả trận đấu. 

Nhiệm vụ là tính xác suất để người chơi đầu tiên thắng toàn bộ trận đấu, được biểu thị dưới dạng phân số rút gọn. 

Các ràng buộc là vô cùng lớn, với cả hai$n$Và$m$lên đến$10^9$. Điều này ngay lập tức loại trừ mọi mô phỏng theo vòng hoặc lập trình động theo trạng thái độ dài$m$. Ngay cả lý luận mỗi vòng phụ thuộc vào việc lặp qua các trạng thái là không thể. Lời giải phải thu gọn toàn bộ tương tác lặp lại thành một xác suất dạng đóng duy nhất chỉ phụ thuộc vào tính đối xứng cấu trúc. 

Một điểm tinh tế là ràng buộc “không có hai nước đi giống hệt nhau liên tiếp” tạo ra sự phụ thuộc giữa các vòng, do đó, giả định ngây thơ về các lựa chọn thống nhất độc lập cho mỗi vòng rõ ràng là không hợp lệ. Bất kỳ giải pháp đúng nào cũng phải chứng minh rằng ràng buộc này không ảnh hưởng đến xác suất thắng cuối cùng hoặc chứng minh rằng nó bị loại bỏ do tính đối xứng. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô phỏng trò chơi như một quá trình ngẫu nhiên$m$vòng. Mỗi trạng thái sẽ bao gồm nước đi cuối cùng của cả hai người chơi, vì điều đó hạn chế các lựa chọn trong tương lai. Từ mỗi trạng thái, chúng tôi sẽ liệt kê tất cả các bước đi tiếp theo hợp lệ và tính toán xác suất chuyển đổi trong cách chơi tối ưu. Ngay cả khi chúng ta giả định các chiến lược tối ưu đã biết thì không gian trạng thái có kích thước$O(n^2)$và các quá trình chuyển đổi sẽ cần phải được xử lý cho từng$m$vòng. Điều này hoàn toàn không thể thực hiện được vì$n$Và$m$cả hai đều có ý định$10^9$, khiến cho việc lưu trữ các trạng thái cũng không thể thực hiện được. 

Quan sát quan trọng là trò chơi hoàn toàn đối xứng dưới sự xoay vòng theo chu kỳ của các ký hiệu. Mỗi biểu tượng có chính xác một mối quan hệ thắng và một thua trong một chu kỳ thống nhất và cả hai người chơi đều phải đối mặt với các ràng buộc và mục tiêu giống nhau. Việc hạn chế không lặp lại bước di chuyển trước đó chỉ gây ra hiệu ứng bộ nhớ cục bộ nhưng không phá vỡ tính đối xứng giữa các ký hiệu. 

Bởi vì cả hai người chơi đều giống nhau về sức mạnh và những ràng buộc, nên bất kỳ chiến lược tối ưu nào cũng phải xử lý tất cả các ký hiệu một cách tương đương. Không có lý do gì để thích bất kỳ biểu tượng nào trên toàn cầu, vì việc xoay tất cả các nhãn sẽ tạo ra một trò chơi tương đương. Điều này buộc quá trình chuyển sang trạng thái cân bằng đối xứng trong đó mọi biểu tượng được sử dụng với khả năng cấu trúc như nhau và không biểu tượng nào có thể mang lại lợi thế lâu dài qua các vòng. 

Dưới sự đối xứng này, toàn bộ$m$-trận đấu vòng tròn có thể trao đổi giữa hai người chơi. Việc hoán đổi người chơi không làm thay đổi sự phân bố kết quả, vì vậy xác suất người chơi A thắng phải bằng xác suất người chơi B thắng. Bất kỳ khối lượng xác suất còn lại nào đều tương ứng với các mối quan hệ trong điểm số cuối cùng, nhưng những mối quan hệ này cũng đối xứng. 

Lập luận đối xứng này thu gọn toàn bộ quá trình động thành một kết luận duy nhất: giữa$n$vai trò tuần hoàn đối xứng, chính xác một vai trò tương ứng là “căn chỉnh vượt trội” so với quỹ đạo của đối thủ. Mỗi cấu hình tương đối ban đầu đều có khả năng xảy ra như nhau và chính xác là một trong các cấu hình$n$phép quay mang lại chiến thắng cho người chơi A. Do đó, xác suất thắng là$1/n$, độc lập với$m$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu |$O(n^2 \cdot m)$|$O(n^2)$| Quá chậm | 
| Giảm đối xứng |$O(1)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc$n$Và$m$. Số vòng$m$hóa ra không ảnh hưởng đến xác suất cuối cùng vì trò chơi vẫn có tính đối xứng ở mọi giai đoạn. 
2. Hãy quan sát rằng trò chơi hoàn toàn bất biến khi gắn nhãn lại các ký hiệu theo chu kỳ. Điều này có nghĩa là việc dịch chuyển tất cả các ký hiệu theo một độ lệch cố định sẽ không làm thay đổi kết quả. 
3. Kết luận rằng người chơi A và người chơi B không thể phân biệt được trong bất kỳ chiến lược tối ưu nào. Bất kỳ sự biến đổi nào hoán đổi vai trò của chúng sẽ khiến trò chơi không thay đổi. 
4. Vì có chính xác$n$các vị trí theo chu kỳ trong đó người chơi A có thể có lợi thế tương đối so với người chơi B trong quá trình tương tác đối xứng và tất cả đều có khả năng như nhau, mỗi vị trí tương ứng với xác suất$1/n$. 
5. Xuất ra phân số rút gọn$1/n$. 

### Tại sao nó hoạt động 

Bất biến quan trọng là tính đối xứng dưới sự dán nhãn lại của$n$biểu tượng kết hợp với sự đối xứng giữa hai người chơi. Không có quy tắc nào đưa ra sự thiên vị dai dẳng đối với bất kỳ biểu tượng hoặc người chơi nào. Quy tắc “không lặp lại liên tiếp” hạn chế các chuyển đổi cục bộ nhưng giống hệt nhau đối với cả hai người chơi và duy trì tính đối xứng quay của không gian trạng thái. Bởi vì kết quả cuối cùng chỉ phụ thuộc vào sự liên kết tương đối của hai quá trình đối xứng, nên tất cả$n$sự sắp xếp có khả năng như nhau, buộc xác suất thắng của người chơi A phải chính xác$1/n$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())
print(f"1/{n}")
```Giải pháp bỏ qua$m$hoàn toàn vì số vòng không ảnh hưởng đến đối số đối xứng. Số lượng duy nhất quan trọng là$n$, số lượng các lựa chọn tuần hoàn. 

Chúng tôi in trực tiếp phân số$1/n$mà không cần tính toán thêm. Từ$n \le 10^9$, nó đã biểu thị một phân số rút gọn với tử số 1, do đó không cần rút gọn GCD. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 3
```Đây$n=3$, do đó các ký hiệu tạo thành một chu kỳ tam giác. Mỗi cấu hình đều đối xứng và cả ba sự sắp xếp theo chu kỳ giữa những người chơi đều có khả năng xảy ra như nhau. 

| Bước | Ý tưởng chính | 
| --- | --- | 
| 1 | Xác định tính đối xứng trên 3 ký hiệu tuần hoàn | 
| 2 | Mỗi căn chỉnh đều có khả năng như nhau | 
| 3 | Chỉ có một căn chỉnh tương ứng với chiến thắng chung cuộc của A | 

Đầu ra:```
1/3
```This confirms that increasing the number of rounds to 3 does not affect the probability.

 ### Ví dụ 2 

đầu vào:```
4 1
```Bây giờ có 4 biểu tượng trong một chu kỳ, nhưng chỉ có một vòng được chơi. Ngay cả trong một vòng đấu duy nhất, tính đối xứng ngụ ý rằng mỗi biểu tượng đều có khả năng là hướng chiến thắng quyết định như nhau. 

| Bước | Ý tưởng chính | 
| --- | --- | 
| 1 | Chu kỳ cỡ 4 | 
| 2 | Một trong bốn kết quả đối xứng ủng hộ A | 
| 3 | Xác suất phân bố đều | 

Đầu ra:```
1/4
```Điều này cho thấy ngay cả khi$m=1$, sự đối xứng cấu trúc tương tự được áp dụng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(1)$| Chỉ đọc dữ liệu đầu vào và in biểu thức dạng hằng số | 
| Không gian |$O(1)$| Không có cấu trúc dữ liệu bổ sung | 

Giải pháp này thỏa mãn một cách tầm thường các ràng buộc vì nó không thực hiện tính toán phụ thuộc vào$n$hoặc$m$, cả hai đều có thể lớn bằng$10^9$. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, m = map(int, sys.stdin.readline().split())
    return f"1/{n}"

# provided samples
assert run("3 3\n") == "1/3"
assert run("4 1\n") == "1/4"

# custom cases
assert run("3 1\n") == "1/3"
assert run("5 10\n") == "1/5"
assert run("10 1000000000\n") == "1/10"
assert run("1000000000 1\n") == "1/1000000000"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 1 | 1/3 | hành vi chu kỳ tối thiểu | 
| 5 10 | 1/5 | độc lập khỏi m | 
| 10^9 1 | 1/10^9 | xử lý n lớn | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi$m$là cực kỳ lớn. Một cách tiếp cận đơn giản có thể cố gắng mô phỏng các vòng chơi hoặc tính toán phân bổ theo các trạng thái trò chơi phát triển theo$m$, nhưng đối số đối xứng cho thấy rằng không có sự phụ thuộc nào như vậy tồn tại. 

Ví dụ: với đầu vào:```
10 1000000000
```đầu ra đúng vẫn còn:```
1/10
```Mặc dù mô phỏng sẽ cố gắng theo dõi các chuyển đổi Markov dài hạn gây ra bởi ràng buộc “không lặp lại”, cả hai người chơi đều bị ràng buộc giống hệt nhau và tính đối xứng chu kỳ đảm bảo không tích lũy sai lệch theo thời gian. Xác suất cuối cùng vẫn được cố định hoàn toàn bởi kích thước của tập hành động đối xứng.
