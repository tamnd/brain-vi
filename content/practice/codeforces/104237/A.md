---
title: "CF 104237A - Thú vị với việc kiểm tra thực phẩm"
description: "Bốn thùng rác được đặt xung quanh khuôn viên trường và vào cuối bữa trưa, mỗi thùng sẽ được cân. Mỗi thùng có trọng lượng rỗng đã biết là 6 pound và dữ liệu đầu vào sẽ đưa ra tổng trọng lượng đo được của mỗi thùng sau khi sử dụng."
date: "2026-07-01T23:19:29+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104237
codeforces_index: "A"
codeforces_contest_name: "Harker Programming Invitational 2023 Novice"
rating: 0
weight: 104237
solve_time_s: 70
verified: true
draft: false
---

[CF 104237A - Thú vị với việc kiểm tra thực phẩm](https://codeforces.com/problemset/problem/104237/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 10s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Bốn thùng rác được đặt xung quanh khuôn viên trường và vào cuối bữa trưa, mỗi thùng sẽ được cân. Mỗi thùng có trọng lượng rỗng đã biết là 6 pound và dữ liệu đầu vào sẽ đưa ra tổng trọng lượng đo được của mỗi thùng sau khi sử dụng. Mục đích là để xác định có bao nhiêu chất thải thực phẩm bên trong cả bốn thùng cộng lại. 

Quan sát chính là trọng lượng đo được bao gồm hai thành phần: trọng lượng hộp đựng cố định và chất thải thực phẩm có thể thay đổi. Vì mỗi thùng có trọng lượng rỗng giống nhau nên chất thải trong một thùng chỉ đơn giản là trọng lượng đo được của nó trừ đi sáu. Câu trả lời cuối cùng là tổng giá trị chất thải trên mỗi thùng trên cả bốn thùng. 

Kích thước đầu vào được cố định ở chính xác bốn số nguyên, do đó không có vấn đề gì về tỷ lệ. Ngay cả trong cách giải thích tổng quát nhất, việc tính toán diễn ra theo thời gian không đổi và chỉ cần một vài phép tính số học. Điều này loại trừ mọi nhu cầu về vòng lặp trên các cấu trúc lớn, cấu trúc dữ liệu hoặc kỹ thuật tối ưu hóa. Một giải pháp đúng hoàn toàn là số học. 

Trường hợp cạnh chính là tràn ngầm nếu ai đó quên trừ trọng lượng trống trên mỗi thùng và thay vào đó trừ nó một lần trên toàn bộ hoặc hoàn toàn không trừ. Ví dụ: nếu tất cả các thùng đều trống và mỗi thùng nặng 6 thì câu trả lời đúng là 0. Một tổng đơn giản không có phép trừ sẽ báo cáo sai 24. 

Một dạng sai sót tinh vi khác là trộn lẫn cách giải thích và trừ không chính xác trên tổng số thay vì trên mỗi thùng. Ví dụ, đưa ra`18 10 9 25`, chất thải đúng là`(18-6) + (10-6) + (9-6) + (25-6) = 12 + 4 + 3 + 19 = 38`. Nếu một người chỉ trừ sáu một lần từ tổng số tiền`62 - 6`, kết quả trở thành 56, sai vì trọng lượng rỗng áp dụng độc lập cho mỗi thùng. 

## Phương pháp tiếp cận 

Cách trực tiếp để suy nghĩ về vấn đề là mô phỏng tình huống theo nghĩa đen. Chúng tôi đọc bốn trọng lượng, giả sử mỗi trọng lượng bao gồm 6 pound khối lượng thùng chứa và đối với mỗi thùng, chúng tôi tính hàm lượng thực phẩm bằng phép trừ, sau đó tính tổng chúng. Đây đã là giải pháp đầy đủ. 

Một cách đóng khung đơn giản hơn sẽ là tách tổng trọng lượng của mỗi thùng thành “hộp đựng + thực phẩm” một cách rõ ràng và cố gắng tích lũy khối lượng hộp đựng và khối lượng thực phẩm một cách riêng biệt. Điều đó vẫn dẫn đến cách tính toán tương tự nhưng dẫn đến việc ghi sổ kế toán không cần thiết. Vì mỗi container đều có trọng lượng đã biết giống nhau nên không cần phải mô hình hóa chúng một cách riêng biệt. 

Thông tin chi tiết quan trọng là sự đóng góp của vùng chứa được lặp lại liên tục bốn lần. Thay vì suy nghĩ theo cách phân tách tổng trọng lượng, chúng ta có thể trừ trực tiếp chi phí cố định cho mỗi mặt hàng. Điều này làm giảm vấn đề thành một lần tuyến tính duy nhất trên bốn giá trị với số học không đổi cho mỗi phần tử. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Trừ trực tiếp trên mỗi thùng | O(1) | O(1) | Đã chấp nhận | 
| Phân hủy dư thừa | O(1) | O(1) | Được chấp nhận nhưng không cần thiết | 

## Hướng dẫn thuật toán 

1. Đọc bốn số nguyên biểu thị trọng lượng đo được của các thùng. Mỗi giá trị đã bao gồm cả hộp đựng và khối lượng thực phẩm. 
2. Đối với mỗi trọng lượng, hãy trừ đi 6 để chỉ tách riêng rác thực phẩm bên trong thùng đó. Bước này được thực hiện độc lập trên mỗi thùng vì mỗi thùng chứa có trọng lượng cố định riêng. 
3. Cộng bốn giá trị chất thải đã tính toán lại với nhau để có được tổng lượng chất thải thực phẩm trên toàn khuôn viên trường. 
4. Xuất kết quả tổng. 

### Tại sao nó hoạt động 

Mỗi giá trị đo được có thể được biểu diễn dưới dạng`w_i = 6 + f_i`, Ở đâu`f_i`là rác thải thực phẩm ở thùng i. Tổng hợp cả bốn cho`Σ w_i = 24 + Σ f_i`. Trừ 24 hoàn toàn tương đương với việc trừ sáu từ mỗi thùng riêng lẻ. Thuật toán thực hiện việc phân tách cục bộ trên mỗi phần tử, đảm bảo thành phần cố định được loại bỏ chính xác một lần trên mỗi thùng và chỉ để lại tổng lượng chất thải. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

weights = list(map(int, input().split()))
total_waste = 0

for w in weights:
    total_waste += (w - 6)

print(total_waste)
```Giải pháp đọc bốn số nguyên trong một dòng, điều này an toàn vì kích thước đầu vào là cố định. Mỗi giá trị được xử lý độc lập, trừ đi trọng lượng rỗng không đổi là 6 pound. 

Vòng lặp là cách rõ ràng nhất để thể hiện sự phân hủy trên mỗi thùng. Mặc dù có thể hủy cuộn vòng lặp đối với bốn giá trị, nhưng việc giữ nó rõ ràng sẽ tránh được mã hóa cứng và làm cho logic rõ ràng hơn. 

Việc trừ được thực hiện trước khi tích lũy, điều này đảm bảo chúng tôi không bao giờ tổng hợp nhầm trọng lượng vùng chứa vào tổng cuối cùng. Vì tất cả đầu vào được đảm bảo có ít nhất một, nên việc trừ sáu có thể tạo ra giá trị trung gian âm chỉ trong những trường hợp vô nghĩa về mặt vật lý, nhưng trong giới hạn cho phép thì nó vẫn an toàn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
18 10 9 25
```| Thùng | Cân nặng | Tính toán chất thải | Chất thải | 
| --- | --- | --- | --- | 
| 1 | 18 | 18 - 6 | 12 | 
| 2 | 10 | 10 - 6 | 4 | 
| 3 | 9 | 9 - 6 | 3 | 
| 4 | 25 | 25 - 6 | 19 | 

Tổng lượng chất thải = 12 + 4 + 3 + 19 = 38 

Điều này xác nhận rằng mỗi thùng được xử lý độc lập và trọng lượng thùng chứa không đổi được loại bỏ cho mỗi mặt hàng. 

### Ví dụ 2 

đầu vào:```
6 6 6 6
```| Thùng | Cân nặng | Tính toán chất thải | Chất thải | 
| --- | --- | --- | --- | 
| 1 | 6 | 6 - 6 | 0 | 
| 2 | 6 | 6 - 6 | 0 | 
| 3 | 6 | 6 - 6 | 0 | 
| 4 | 6 | 6 - 6 | 0 | 

Tổng lượng chất thải = 0 

Trường hợp này xác minh điều kiện biên trong đó không có thực phẩm và tất cả các thùng đều khớp chính xác với trọng lượng rỗng của chúng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chính xác bốn phép tính số học và bốn phép trừ bất kể giá trị đầu vào | 
| Không gian | O(1) | Chỉ có một số lượng biến số nguyên không đổi được lưu trữ | 

Việc tính toán diễn ra theo thời gian không đổi vì kích thước đầu vào là cố định. Nó dễ dàng đáp ứng mọi ràng buộc thực tế, bao gồm cả giới hạn thời gian nghiêm ngặt. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    weights = list(map(int, input().split()))
    print(sum(w - 6 for w in weights))

# provided sample
assert run("18 10 9 25") == "38\n", "sample 1"

# all empty bins
assert run("6 6 6 6") == "0\n", "all empty"

# single heavy bin
assert run("6 6 6 100") == "94\n", "one full bin"

# minimal allowed weights
assert run("1 1 1 1") == "-20\n", "below empty weight edge"

# mixed values
assert run("7 8 9 10") == "10\n", "mixed small values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 18 10 9 25 | 38 | độ chính xác của mẫu | 
| 6 6 6 6 | 0 | ranh giới thùng rỗng | 
| 6 6 6 100 | 94 | thùng thống trị duy nhất | 
| 1 1 1 1 | -20 | hành vi trừ tối thiểu | 
| 7 8 9 10 | 10 | trường hợp hỗn hợp chung | 

## Vỏ cạnh 

Trường hợp cạnh quan trọng nhất là khi tất cả các thùng đều trống. Đối với đầu vào`6 6 6 6`, mỗi phép trừ tạo ra số 0 và tổng cuối cùng vẫn bằng 0. Thuật toán xử lý từng thùng một cách độc lập nên không xảy ra lỗi tích lũy ẩn. 

Một trường hợp khác là khi trọng số gần với giá trị tối thiểu cho phép. Đối với đầu vào`1 1 1 1`, mỗi thùng đóng góp`1 - 6 = -5`, dẫn đến tổng cộng`-20`. Việc tính toán vẫn tuân theo cấu trúc số học đã xác định mặc dù việc giải thích vật lý trở nên vô nghĩa. Thuật toán không giả định tính không âm; nó chỉ đơn giản là áp dụng phép biến đổi đã cho một cách nhất quán. 

Trường hợp cạnh cuối cùng là phân phối sai lệch, chẳng hạn như`6 6 6 100`. Chỉ có một thùng tạo ra chất thải khác 0 và phần còn lại hủy bỏ chính xác về 0. Phép trừ trên mỗi ngăn đảm bảo không có sự ghép nối giữa các giá trị, do đó các giá trị ngoại lệ lớn không ảnh hưởng đến tính chính xác của các ngăn khác.
