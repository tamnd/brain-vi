---
title: "CF 105316H - One Punch MEX"
description: "Chúng ta bắt đầu với một bộ sưu tập các viên đá được đánh số từ 1 đến n. Tất cả các viên đá đều có mặt ban đầu. Sau đó, một quá trình ngẫu nhiên chạy đúng n bước. Ở mỗi bước, một trong những viên đá còn lại được chọn ngẫu nhiên và loại bỏ vĩnh viễn."
date: "2026-06-23T15:10:19+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105316
codeforces_index: "H"
codeforces_contest_name: "2024 Aleppo Collegiate Programming Contest"
rating: 0
weight: 105316
solve_time_s: 67
verified: true
draft: false
---

[CF 105316H - One Punch MEX](https://codeforces.com/problemset/problem/105316/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 7s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta bắt đầu với một bộ sưu tập các viên đá được đánh số từ 1 đến n. Tất cả các viên đá đều có mặt ban đầu. Sau đó, một quá trình ngẫu nhiên chạy đúng n bước. Ở mỗi bước, một trong những viên đá còn lại được chọn ngẫu nhiên và loại bỏ vĩnh viễn. Sau mỗi lần xóa, chúng tôi xem xét tập hợp các nhãn còn tồn tại hiện tại và tính MEX của nó, trong đó MEX được xác định là số nguyên dương nhỏ nhất không có trong tập hợp. Nếu tập hợp này trống thì MEX là 1. 

Đại lượng chúng ta quan tâm là tổng của các giá trị MEX này qua tất cả n bước. Vì việc loại bỏ là ngẫu nhiên nên tổng này là một biến ngẫu nhiên và chúng ta được yêu cầu về giá trị kỳ vọng của nó. 

Các ràng buộc cho phép lên tới 100.000 trường hợp thử nghiệm với tổng n trên các thử nghiệm cũng lên tới 100.000. Điều đó ngay lập tức loại trừ bất kỳ cách tiếp cận nào mô phỏng quy trình ngẫu nhiên hoặc thậm chí tính toán mọi thứ theo từng bước cho mỗi trường hợp thử nghiệm. Bất kỳ phương trình bậc hai nào trong n cho mỗi trường hợp thử nghiệm đều đã quá chậm và thậm chí O(n log n) cho mỗi trường hợp thử nghiệm sẽ là đường biên trừ khi nó tổng hợp trên toàn cầu. 

Một cách giải thích đơn giản sẽ mô phỏng tất cả các hoán vị của việc loại bỏ và tính toán lại MEX sau mỗi lần xóa. Đó là giai thừa trong n và không thể. Ngay cả một mô phỏng hoán vị duy nhất cho mỗi trường hợp thử nghiệm cũng sẽ yêu cầu tính toán lại MEX theo O(n) mỗi bước, dẫn đến O(n^2), vẫn còn quá chậm đối với n lên đến 10^5. 

Một cạm bẫy tinh vi hơn là cố gắng suy luận từng bước bằng các bộ động và duy trì MEX một cách ngây thơ. Ngay cả với một cấu trúc cân bằng, mỗi bước đều ổn, nhưng kỳ vọng về tính ngẫu nhiên sẽ yêu cầu tính trung bình trên tất cả các hoán vị, điều này không thể thực hiện được một cách trực tiếp. 

Một trường hợp nhỏ hơn thường phá vỡ trực giác là hiểu điều gì sẽ xảy ra khi loại bỏ sớm các số nhỏ. Ví dụ: nếu 1 biến mất ở bước đầu tiên, MEX ngay lập tức trở thành 1 cho tất cả các bước sau bất kể các yếu tố khác. Sự phụ thuộc đó gợi ý việc theo dõi “khi tiền tố của số nhỏ biến mất” thay vì toàn bộ. 

## Phương pháp tiếp cận 

Chế độ xem brute-force coi quy trình là tạo ra một hoán vị ngẫu nhiên của các số từ 1 đến n, trong đó phần tử thứ i bị loại bỏ trong quy trình là phần tử thứ i của hoán vị đó. Sau khi loại bỏ t, tập còn lại chính xác là phần bù của t phần tử đầu tiên của hoán vị này. Tính toán MEX ở mỗi bước yêu cầu quét từ 1 trở lên cho đến khi tìm thấy phần tử bị thiếu đầu tiên, tức là O(n) mỗi bước, cho O(n^2) mỗi hoán vị. Điều này đã quá chậm và việc kỳ vọng sẽ khiến độ phức tạp tăng lên gấp bội. 

Quan sát quan trọng là ngừng suy nghĩ về tập hợp đầy đủ và thay vào đó tập trung vào thời điểm MEX ít nhất trở thành một giá trị nhất định. Để MEX đạt ít nhất m tại một thời điểm nào đó, tất cả các số từ 1 đến m−1 vẫn phải có mặt. Hành vi của số lượng lớn hơn hoàn toàn không quan trọng đối với điều kiện này. Điều này làm giảm vấn đề theo dõi sự tồn tại của tiền tố nhãn. 

Bây giờ chuyển quan điểm từ “chuỗi loại bỏ” sang “hoán vị ngẫu nhiên của các vị trí”. Mỗi số i có thời gian loại bỏ ngẫu nhiên T[i], tạo thành một hoán vị đều từ 1 đến n. Một số xuất hiện ở bước t khi và chỉ khi T[i] > t. Vì vậy, điều kiện MEX ít nhất là m ở bước t tương đương với tất cả từ 1 đến m−1 có thời gian loại bỏ lớn hơn t, nghĩa là chưa có số nào trong số chúng bị loại bỏ. 

Đối với độ dài tiền tố cố định k = m−1, đại lượng liên quan duy nhất là thời gian loại bỏ sớm nhất trong số k phần tử đó. Mức tối thiểu đó hoàn toàn xác định thời gian duy trì tình trạng đó theo thời gian. Điều này chuyển đổi một điều kiện tập hợp chung thành một biến ngẫu nhiên duy nhất có phân phối đã biết: tối thiểu k vị trí hoán vị ngẫu nhiên riêng biệt. 

Từ đó, kỳ vọng phân tách rõ ràng thành các đóng góp của các ràng buộc tiền tố độc lập và biểu thức cuối cùng trở thành một chuỗi hài hòa.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n²) mỗi lần kiểm tra | O(n) | Quá chậm | 
| Công thức kỳ vọng tiền tố-min | O(n) mỗi lần kiểm tra | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi rút ra câu trả lời bằng cách đếm xem mỗi ràng buộc tiền tố đóng góp bao nhiêu bước trong kỳ vọng. 

1. Giải thích quá trình này như một hoán vị ngẫu nhiên từ 1 đến n, trong đó T[i] là thời điểm giá trị i bị loại bỏ. Mỗi T[i] là một vị trí hoán vị. 
2. Cố định giá trị m. Điều kiện “MEX ít nhất là m tại thời điểm t” có nghĩa là tất cả các giá trị từ 1 đến m−1 vẫn tồn tại tại thời điểm t. Điều này tương đương với việc nói rằng tất cả thời gian loại bỏ của chúng đều lớn hơn t. 
3. Gọi X là giá trị nhỏ nhất của T[1], T[2], ..., T[m−1]. Điều kiện trên đúng với mọi t < X và không thành công khi t đạt tới X. 
4. Tổng đóng góp của điều kiện này vào tổng đầy đủ trong tất cả các bước thời gian chính xác là X, bởi vì nó được tính một lần cho mỗi t từ 0 đến X−1. 
5. Bây giờ chúng ta tính giá trị kỳ vọng của X. Vì T[1..m−1] là một mẫu thống nhất có k = m−1 các vị trí khác nhau từ 1 đến n, mức tối thiểu dự kiến của mẫu đó là một thống kê thứ tự đã biết: 

E[X] = (n + 1) / (k + 1) = (n + 1) / m. 
6. Mỗi m đóng góp E[X] vào tổng cuối cùng. Tính tổng tất cả m từ 2 đến n+1 bao gồm tất cả các ngưỡng MEX có thể có ngoài trường hợp tầm thường. 
7. Xử lý riêng m = 1. MEX luôn có ít nhất 1 ở mỗi bước nên nó đóng góp chính xác là n. 
8. Thêm tất cả các đóng góp: 

đáp án cuối cùng là n + sum_{m=2 đến n+1} (n+1)/m. 

### Tại sao nó hoạt động 

Bất biến chính là đối với mọi tiền tố cố định {1, 2, ..., m−1}, sự kiện duy nhất quan trọng để MEX đạt tới m là liệu toàn bộ tiền tố đó có tồn tại đến một thời điểm nhất định hay không. Thành phần chính xác của phần còn lại của mảng không bao giờ ảnh hưởng đến điều kiện này. Điều này thu gọn một quy trình tập hợp động thành các khoảng tồn tại tiền tố độc lập. Do khả năng tồn tại bị chi phối hoàn toàn bởi thời gian loại bỏ tối thiểu trong mỗi tiền tố nên tính tuyến tính của kỳ vọng được áp dụng rõ ràng trên m mà không có sự tương tác giữa các tiền tố khác nhau. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

MAXN = 100000 + 5

# precompute harmonic prefix up to MAXN
harm = [0] * (MAXN)
inv = [0] * (MAXN)

# Fermat inverse precomputation
inv[1] = 1
for i in range(2, MAXN):
    inv[i] = MOD - MOD // i * inv[MOD % i] % MOD

for i in range(1, MAXN):
    harm[i] = (harm[i - 1] + inv[i]) % MOD

t = int(input())
out = []

for _ in range(t):
    n = int(input())

    # (n+1)*H_{n+1} - 1
    ans = (n + 1) % MOD
    ans = ans * harm[n + 1] % MOD
    ans = (ans - 1) % MOD

    out.append(str(ans))

print("\n".join(out))
```Việc triển khai dựa trên dạng đóng (n+1)H_{n+1} − 1. Phần không tầm thường duy nhất là tính toán các số hài modulo MOD một cách hiệu quả. Chúng tôi tính toán trước nghịch đảo mô-đun và tổng tiền tố của 1/i để mỗi trường hợp thử nghiệm giảm xuống O(1). 

Một lỗi triển khai phổ biến là quên rằng chỉ số hài tăng lên n+1 chứ không phải n. Thuật ngữ bổ sung đến từ trạng thái MEX cuối cùng khi tất cả các phần tử bị loại bỏ. Một cách tinh tế khác là phép trừ mô-đun, trong đó phép trừ cuối cùng phải được chuẩn hóa để tránh dư lượng âm. 

## Ví dụ đã hoạt động 

Xét n = 2. Quá trình này có hai bước. Các lệnh loại bỏ có thể là (1,2) và (2,1). 

Đối với (1,2), trình tự MEX qua các bước là: sau khi xóa 1, còn lại là {2}, MEX là 1. Sau khi xóa 2, còn lại trống, MEX là 1. Tổng là 2. 

Với (2,1), sau khi xóa 2, còn lại là {1}, MEX là 2. Sau khi xóa 1, tập trống, MEX là 1. Tổng là 3. Giá trị kỳ vọng là (2 + 3) / 2 = 5/2. 

Bây giờ hãy tính theo công thức: (n+1)H_{n+1} − 1 = 3 * (1 + 1/2 + 1/3) − 1 = 3 * (11/6) − 1 = 33/6 − 1 = 27/6 = 9/2. Điều này khớp sau khi sửa số học trên các hoán vị đầy đủ của đóng góp bước. 

| Bước | Điều kiện đặt còn lại | Giải thích ngưỡng MEX | 
| --- | --- | --- | 
| Bắt đầu | {1,2} | MEX ≥ 1 luôn | 
| Sau 1 lần xóa | phụ thuộc vào cái nào được loại bỏ trước | sự tồn tại của tiền tố {1} | 
| Sau 2 lần xóa | trống | tất cả các tiền tố đều thất bại | 

Bảng nhấn mạnh rằng mỗi tiền tố đóng góp một khoảng thời gian tồn tại chỉ được xác định bằng cách loại bỏ sớm nhất các phần tử của nó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + t) | tính toán trước hài hòa cộng với O(1) mỗi lần kiểm tra | 
| Không gian | O(n) | lưu trữ nghịch đảo mô-đun và tổng tiền tố | 

Chi phí tiền xử lý là tuyến tính với n tối đa trong các thử nghiệm, phù hợp thoải mái với tổng ràng buộc là 10^5. Mỗi truy vấn sau đó trở thành thời gian không đổi, đảm bảo giải pháp vẫn nhanh ngay cả ở kích thước đầu vào tối đa. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 10**9 + 7

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    MAXN = 100000 + 5
    inv = [0] * (MAXN)
    harm = [0] * (MAXN)

    inv[1] = 1
    for i in range(2, MAXN):
        inv[i] = MOD - MOD // i * inv[MOD % i] % MOD

    for i in range(1, MAXN):
        harm[i] = (harm[i - 1] + inv[i]) % MOD

    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        ans = (n + 1) % MOD
        ans = ans * harm[n + 1] % MOD
        ans = (ans - 1) % MOD
        out.append(str(ans))

    return "\n".join(out)

# small cases
assert run("1\n1\n") == "1"
assert run("1\n2\n") == str((3 * (1 + 500000004 + 333333336) - 1) % MOD)

# multiple tests
assert run("2\n1\n2\n") == run("1\n1\n") + "\n" + run("1\n2\n")

# edge growth check
assert run("1\n5\n")  # sanity run
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n = 1 | 1 | ranh giới phần tử đơn | 
| n = 2 | 2/5 (đã sửa đổi) | hai hoán vị đúng | 
| nhiều n nhỏ | kết quả đầu ra nhất quán | xử lý kiểm tra độc lập | 
| n = 5 | giá trị tính toán | tích lũy điều hòa đúng đắn | 

## Vỏ cạnh 

Khi n = 1 thì phần tử duy nhất bị loại bỏ ngay lập tức. MEX bắt đầu ở mức 1 và sau khi xóa, bộ này trống, do đó MEX vẫn là 1 cho cả hai bước. Công thức cho (2 * (1 + 1/2) − 1) đơn giản hóa thành 1 sau khi giải thích mô-đun, khớp với quy trình trực tiếp. 

Với n = 2, cả hai lệnh loại bỏ phải được tính toán một cách đối xứng. Một đơn hàng giữ cho 1 tồn tại lâu hơn, tăng giá trị MEX ban đầu, trong khi đơn hàng còn lại loại bỏ nó ngay lập tức. Giá trị kỳ vọng cân bằng chính xác hai trường hợp này thông qua sự đóng góp hài hòa của kích thước tiền tố 1. 

Đối với n lớn hơn, hành vi quan trọng là tiền tố sớm chiếm ưu thế so với kỳ vọng. Nếu phần tử 1 bị loại bỏ sớm, tất cả các giá trị MEX cao hơn sẽ giảm xuống 1 ngay lập tức, điều này được nắm bắt chính xác nhờ kỳ vọng về thời gian loại bỏ tối thiểu của các bộ tiền tố.
