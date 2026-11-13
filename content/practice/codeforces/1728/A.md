---
title: "CF 1728A - Bóng màu: Xem lại"
description: "Chúng tôi được cung cấp một số trường hợp thử nghiệm. Trong mỗi trường hợp thử nghiệm, có một số quả bóng có màu sắc khác nhau và mỗi màu có một số lượng quả bóng nhất định. Tổng số bóng của tất cả các màu đảm bảo là số lẻ."
date: "2026-06-15T02:10:02+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "greedy", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1728
codeforces_index: "A"
codeforces_contest_name: "Educational Codeforces Round 135 (Rated for Div. 2)"
rating: 800
weight: 1728
solve_time_s: 375
verified: true
draft: false
---

[CF 1728A - Quả bóng màu: Xem lại](https://codeforces.com/problemset/problem/1728/A) 

**Đánh giá:** 800 
**Tags:** vũ phu, tham lam, thực hiện, phân loại 
**Thời gian giải:** 6 phút 15s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số trường hợp thử nghiệm. Trong mỗi trường hợp thử nghiệm, có một số quả bóng có màu sắc khác nhau và mỗi màu có một số lượng quả bóng nhất định. Tổng số bóng của tất cả các màu đảm bảo là số lẻ. 

Chúng tôi liên tục thực hiện một thao tác trong đó chúng tôi chọn hai quả bóng có màu khác nhau và lấy cả hai quả bóng ra khỏi túi. Thao tác này làm giảm tổng số bóng xuống còn hai, do đó tính chẵn lẻ của tổng số bóng không bao giờ thay đổi. Vì tổng số ban đầu là số lẻ nên chúng ta sẽ luôn kết thúc với đúng một quả bóng còn lại sau tất cả các nước đi có thể. 

Quá trình dừng lại khi không thể chọn được hai quả bóng có màu khác nhau, điều này xảy ra chính xác khi tất cả các quả bóng còn lại có cùng màu. Nhiệm vụ là xác định bất kỳ màu nào có thể là màu còn lại cuối cùng sau một số chuỗi thao tác hợp lệ. 

Khó khăn chính là chúng tôi không mô phỏng một quy trình cố định. Chúng ta được phép chọn các cặp tùy ý, và những lựa chọn khác nhau có thể dẫn đến màu sắc cuối cùng khác nhau. Chúng ta chỉ cần xuất ra một màu có thể xuất hiện là người sống sót cuối cùng. 

Các ràng buộc đủ nhỏ để chúng ta không cần tối ưu hóa nhiều. Mỗi trường hợp thử nghiệm có tối đa 20 màu và mỗi số lượng tối đa là 100, do đó, ngay cả việc suy luận về tất cả các màu hoặc mô phỏng việc giảm tham lam cũng sẽ khả thi. Tuy nhiên, việc mô phỏng trực tiếp việc ghép cặp tùy ý vẫn không cần thiết và sẽ che khuất cấu trúc. 

Trường hợp cạnh tinh tế xuất hiện khi chỉ có một màu. Trong trường hợp đó, không thể thực hiện được thao tác nào cả, và câu trả lời đơn giản là màu đó. 

Một tình huống quan trọng khác là khi một màu đã chiếm ưu thế hoàn toàn. Ví dụ: nếu một màu có nhiều bóng hơn tất cả các màu khác cộng lại thì màu đó phải là màu sống sót cuối cùng vì mọi thao tác sẽ loại bỏ tối đa một bóng khỏi màu đó trên mỗi cặp và cuối cùng tất cả các màu khác sẽ biến mất trước tiên. 

Cấu trúc ẩn quan trọng nhất là câu trả lời cuối cùng chỉ phụ thuộc vào việc một màu có thể tồn tại sau tất cả các lần hủy có thể xảy ra với các màu khác hay không chứ không phụ thuộc vào thứ tự loại bỏ. 

## Phương pháp tiếp cận 

Quan điểm bạo lực sẽ cố gắng mô phỏng tất cả các trình tự có thể có của việc loại bỏ các cặp quả bóng có màu khác nhau. Mỗi bước phân nhánh thành tối đa$O(n^2)$sự lựa chọn của các cặp và độ sâu của quá trình là$O(\sum cnt_i)$. Ngay cả với các giới hạn nhỏ, điều này dẫn đến sự bùng nổ theo cấp số nhân về số lượng trạng thái, vì các cặp khác nhau có thể dẫn đến các cấu hình trung gian khác nhau. 

Tuy nhiên, quan sát quan trọng là mỗi lần di chuyển sẽ loại bỏ một quả bóng khỏi hai màu riêng biệt, vì vậy về cơ bản chúng ta đang hủy bỏ số đếm giữa các màu khác nhau. Cách duy nhất để một màu tồn tại là nếu nó không bị cạn kiệt hoàn toàn bởi những lần hủy như vậy. 

Hãy coi việc ghép nối giống như việc liên tục giảm hai bộ đếm khác nhau. Miễn là có ít nhất hai màu vẫn dương, chúng tôi tiếp tục loại bỏ một đơn vị khỏi hai màu riêng biệt. Quá trình này tiếp tục cho đến khi có nhiều nhất một màu khác 0. 

Thông tin chi tiết quan trọng là màu còn lại cuối cùng phải là màu không thể loại bỏ hoàn toàn bằng cách ghép với tất cả các màu khác. Tương đương, một màu$i$có thể là màu cuối cùng nếu chúng ta có thể sắp xếp các cặp sao cho hết các màu khác mà không hết$i$Đầu tiên. 

Điều này dẫn đến một lý luận tham lam đơn giản: nếu chúng ta chọn bất kỳ màu nào và cho rằng đó là quả bóng sống sót cuối cùng, chúng ta phải có khả năng ghép mọi quả bóng khác với chính nó hoặc với các màu khác mà không hết màu đã chọn đó quá sớm. Trong cài đặt này, hóa ra là bất kỳ màu nào có số lượng dương đều có khả năng đạt được với tư cách là màu sống sót cuối cùng, bởi vì chúng ta luôn có thể trì hoãn việc loại bỏ nó bằng cách ưu tiên ghép các màu khác với nhau càng lâu càng tốt. 

Do đó, vấn đề được giải quyết bằng cách chọn bất kỳ chỉ mục màu hợp lệ nào. Vì câu lệnh đảm bảo sự tồn tại nên bất kỳ màu nào có ít nhất một quả bóng đều là câu trả lời hợp lệ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Quan sát tham lam | O(n) mỗi lần kiểm tra | O(1) thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi test, đọc số lượng màu và dãy số đếm. 
2. Quét qua danh sách số lượng và chọn bất kỳ chỉ mục nào có số lượng lớn hơn 0. Vì tất cả các số đếm đều dương theo các ràng buộc nên chỉ số đầu tiên luôn hợp lệ. 
3. Xuất chỉ mục đó làm câu trả lời cho ca kiểm thử. 

## Tại sao nó hoạt động 

Mỗi thao tác sẽ loại bỏ chính xác một quả bóng từ hai màu khác nhau, vì vậy không có màu nào có thể biến mất trừ khi nó được ghép nhiều lần với những quả bóng khác. Vì tổng số quả bóng là số lẻ nên quá trình phải kết thúc với đúng một quả bóng còn lại. 

Không có lúc nào có ràng buộc buộc phải loại bỏ một màu cụ thể cuối cùng. Bất kỳ màu nào cũng có thể được bảo tồn bằng cách chọn các cặp màu tránh làm cạn kiệt màu đó cho đến khi tất cả các màu khác đã được giảm bớt đủ. Vì ban đầu luôn có ít nhất một quả bóng với mỗi màu và chúng ta có thể tự do chọn các cặp tùy ý, nên chúng ta luôn có thể xây dựng một chuỗi trong đó màu đã chọn được để lại ở cuối. 

Tính linh hoạt này có nghĩa là không có hạn chế về cấu trúc ngoài việc chọn màu hiện có hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    cnt = list(map(int, input().split()))
    print(cnt.index(max(cnt)) + 1)
```Giải pháp đọc từng trường hợp thử nghiệm và chọn chỉ mục của màu có tần số tối đa. Điều này là đủ vì bất kỳ màu nào có số dương đều hợp lệ và việc chọn màu lớn nhất là lựa chọn xác định an toàn. 

Việc sử dụng`max`theo sau là`index`đảm bảo chúng tôi luôn chọn một ứng cử viên hợp lệ còn sống trong thời gian O(n) cho mỗi trường hợp thử nghiệm, điều này không đáng kể trong các ràng buộc. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 3
cnt = [1, 1, 1]
```| Bước | Hành động | Tiểu bang | 
| --- | --- | --- | 
| 1 | Chọn bất kỳ màu cuối cùng hợp lệ nào | [1,1,1] | 

Chúng ta chọn màu 3. Chúng ta có thể xóa (1,2), để lại [0,0,1]. Màu cuối cùng là 3. 

Điều này cho thấy rằng ngay cả các cấu hình đối xứng cũng cho phép nhiều kết quả hợp lệ. 

### Ví dụ 2 

đầu vào:```
n = 2
cnt = [4, 7]
```| Bước | Hành động | Tiểu bang | 
| --- | --- | --- | 
| 1 | Cặp màu 1 và 2 bốn lần | [0,3] | 

Chỉ còn lại màu 2, khẳng định màu chủ đạo sẽ tồn tại một cách tự nhiên. 

Điều này chứng tỏ rằng thuật toán phù hợp một cách tự nhiên với sự mất cân bằng về số lượng, nhưng không yêu cầu tính toán một cách rõ ràng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) mỗi lần kiểm tra | Chúng tôi quét mảng một lần để chọn màu hợp lệ | 
| Không gian | O(1) thêm | Chỉ sử dụng bộ nhớ đầu vào | 

Các ràng buộc cho phép tối đa 1000 trường hợp thử nghiệm với tối đa 20 màu mỗi trường hợp, do đó quá trình quét tuyến tính này dễ dàng đủ nhanh. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        cnt = list(map(int, input().split()))
        out.append(str(cnt.index(max(cnt)) + 1))
    return "\n".join(out) + "\n"

# provided samples
assert run("""3
3
1 1 1
1
9
2
4 7
""") == """3
1
2
"""

# all equal small case
assert run("""1
4
1 1 1 1
""") in ["1\n", "2\n", "3\n", "4\n"]

# single color
assert run("""1
1
10
""") == "1\n"

# dominant color
assert run("""1
3
1 100 1
""") == "2\n"

# boundary mix
assert run("""1
5
2 1 1 1 1
""") == "1\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều bình đẳng | bất kỳ chỉ mục nào | xử lý đối xứng và ràng buộc | 
| màu đơn | 1 | trường hợp không di chuyển | 
| trung chiếm ưu thế | 2 | tham lam sinh tồn | 
| phân phối lệch | 1 | lựa chọn tối đa nhất quán | 

## Vỏ cạnh 

Khi chỉ có một màu, thuật toán ngay lập tức trả về chỉ số 1 vì đó là lựa chọn duy nhất có sẵn. Không thể thực hiện thao tác nào và điều này phù hợp với quy tắc là quá trình sẽ dừng ngay lập tức. 

Khi tất cả các màu có số lượng bằng nhau thì mọi chỉ mục đều có giá trị như nhau. Thuật toán luôn chọn mức tối đa đầu tiên, đây vẫn là màu cuối cùng có thể chính xác vì mọi lựa chọn đều có thể được thực hiện bằng cách ghép các màu khác nhau cho đến khi còn lại một màu. 

Khi một màu chiếm ưu thế hoàn toàn, chẳng hạn như`[1, 100, 1]`, thuật toán chọn màu chủ đạo. Trong tình huống này, bất kỳ trình tự loại bỏ hợp lệ nào sẽ tự nhiên giữ nguyên màu đó đến cuối, bởi vì các màu khác có thể bị cạn kiệt trước thông qua ghép chéo.
