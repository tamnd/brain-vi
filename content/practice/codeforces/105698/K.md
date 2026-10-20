---
title: "CF 105698K - Bữa Tiệc Của Kaz"
description: "Chúng ta có n người bạn ngồi trong một bữa tiệc, mỗi người có một loại đồ uống ưa thích duy nhất. Kaz chỉ định cho mỗi người bạn chính xác một ly đồ uống, tạo thành một hoán vị có kích thước n. Một số người bạn có thể nhận ngay đồ uống phù hợp của mình, trong khi những người khác thì không."
date: "2026-06-22T04:58:39+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105698
codeforces_index: "K"
codeforces_contest_name: "OCPC 2024 Summer, Day 5: OCPC Potluck Contest 2"
rating: 0
weight: 105698
solve_time_s: 47
verified: true
draft: false
---

[CF 105698K - Bữa tiệc của Kaz](https://codeforces.com/problemset/problem/105698/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có n người bạn ngồi trong một bữa tiệc, mỗi người có một loại đồ uống ưa thích duy nhất. Kaz chỉ định cho mỗi người bạn chính xác một ly đồ uống, tạo thành một hoán vị có kích thước n. Một số người bạn có thể nhận ngay đồ uống phù hợp của mình, trong khi những người khác thì không. 

Phần thú vị là những gì xảy ra sau nhiệm vụ đầu tiên này. Bất cứ ai đã uống đúng loại đồ uống sẽ rời khỏi hệ thống. Những người còn lại vẫn chưa hài lòng đã bỏ hết đồ uống của mình trở lại bể chung. Những đồ uống này sau đó được xáo trộn ngẫu nhiên và phân phối lại cho những người còn lại. Một lần nữa, bất kỳ ai tình cờ nhận được đồ uống chính xác của mình sẽ rời đi và quá trình này lặp lại. Điều này tiếp tục cho đến khi không còn ai. 

Số lượng chúng tôi được yêu cầu kiểm soát là số vòng xáo trộn dự kiến. Chúng tôi được phép chọn nhiệm vụ ban đầu của đồ uống và chúng tôi muốn tối đa hóa thời gian, theo dự kiến, quá trình "sửa chữa bằng cách xáo trộn" ngẫu nhiên lặp đi lặp lại này sẽ mất bao lâu. 

Đầu vào chính chỉ là n, số lượng người tham gia. Đầu ra bao gồm hai phần: một số thực biểu thị số vòng dự kiến ​​tối đa có thể và một hoán vị mô tả cách chỉ định đồ uống ban đầu. 

Ràng buộc n 1000 có nghĩa là bất kỳ giải pháp nào mô phỏng tính ngẫu nhiên một cách rõ ràng hoặc xem xét các cấu trúc hàm mũ đều không khả thi ngay lập tức. Ngay cả các cấu trúc O(n^2) cũng ổn, nhưng bất cứ điều gì liên quan đến mô phỏng xác suất lặp đi lặp lại hoặc trạng thái động trên các hoán vị sẽ quá chậm hoặc không cần thiết về mặt khái niệm nếu tồn tại cấu trúc dạng đóng. 

Một trường hợp phức tạp xuất hiện khi n = 1. Nếu người duy nhất nhận được đồ uống của riêng mình, họ sẽ rời đi ngay lập tức và quá trình kết thúc mà không có vòng nào. Giả định ngây thơ rằng “luôn có ít nhất một vòng” sẽ thất bại ở đây. 

Với n = 2, cấu trúc trở nên giàu thông tin. Nếu cả hai người được đổi chỗ, quá trình sẽ kết thúc sau đúng một vòng xáo trộn, vì lần xáo trộn tiếp theo sẽ ngay lập tức giải quyết cả hai. Điều này đã gợi ý rằng cấu trúc của hoán vị ban đầu xác định trực tiếp số lượng người tham gia đang hoạt động khi bắt đầu quá trình và trạng thái ban đầu này sẽ chi phối câu trả lời. 

## Phương pháp tiếp cận 

Phối cảnh brute-force bắt đầu bằng cách sửa một hoán vị và cố gắng tính toán số vòng dự kiến theo quy trình ngẫu nhiên được mô tả. Đối với bất kỳ trạng thái nào của k người tham gia còn lại, người ta sẽ mô phỏng rằng một hoán vị ngẫu nhiên thống nhất được áp dụng cho k mục, đếm xem có bao nhiêu điểm cố định xuất hiện, loại bỏ chúng và tiếp tục. Ngay cả khi chúng ta có thể tính toán kỳ vọng cho một hoán vị một cách hiệu quả, thì việc đánh giá tất cả n! hoán vị là không thể. 

Sự đơn giản hóa quan trọng xuất phát từ việc tách vai trò của hoán vị ban đầu khỏi tính ngẫu nhiên của quá trình. Sau nhiệm vụ đầu tiên, điều quan trọng duy nhất là có bao nhiêu người ban đầu “sai”, bởi vì mỗi vòng tiếp theo sẽ phá hủy hoàn toàn cấu trúc bằng cách áp dụng một hoán vị ngẫu nhiên thống nhất mới cho tập hợp còn lại. Việc xác định ai sai là không liên quan, chỉ có kích thước của tập hợp còn lại mới quan trọng. 

Một thực tế xác suất quan trọng là trong một hoán vị ngẫu nhiên đồng nhất của k phần tử, số điểm cố định dự kiến ​​luôn chính xác là 1 với k ≥ 1. Điều này có nghĩa là trong mỗi vòng có ít nhất một người tham gia còn lại, số người dự kiến rời khỏi vòng đó là 1. 

Vì vậy, nếu chúng ta bắt đầu với k người trong hệ thống, mức giảm dự kiến về số lượng người tham gia tích cực mỗi vòng là 1 người mỗi lần, cho đến khi hệ thống trống rỗng. Điều này làm cho số vòng dự kiến ​​​​bằng k.

Do đó, toàn bộ vấn đề tập trung vào việc tối đa hóa k, số người ban đầu sai. Điều này đạt được bằng cách giảm thiểu số lượng điểm cố định trong hoán vị ban đầu. Hoán vị tốt nhất có thể là một hoán vị, trong đó không có chỉ số i nào thỏa mãn p[i] = i, vì vậy k = n. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Xây dựng loạn loạn | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Nếu n bằng 1, xuất ra một hoán vị chỉ chứa 1 và đặt giá trị mong đợi thành 0. Quá trình bắt đầu trống vì người tham gia duy nhất đã đúng, do đó không bao giờ xảy ra sự xáo trộn. 
2. Nếu n lớn hơn 1, hãy xây dựng bất kỳ hoán vị nào không có điểm cố định. Một cấu trúc đơn giản là dịch chuyển mọi chỉ số đi một vị trí, gửi i đến i + 1 và gửi n đến 1. Điều này đảm bảo rằng ban đầu mọi người bạn đều uống nhầm đồ uống. 
3. Xuất ra số vòng dự kiến ​​là n. Điều này xảy ra vì tất cả n người tham gia đều bắt đầu quá trình trao đổi và số lượng người tham gia dự kiến bị loại bỏ mỗi vòng luôn là 1. 
4. Xuất ra hoán vị đã xây dựng. 

Sự lựa chọn thiết kế quan trọng là đảm bảo ban đầu không có điểm cố định. Bất kỳ điểm cố định nào cũng ngay lập tức làm giảm tập hoạt động ban đầu và giảm nghiêm ngặt số vòng dự kiến, vì mỗi điểm cố định là một người tham gia hoàn toàn không tham gia vào quá trình trao đổi. 

### Tại sao nó hoạt động 

Gọi k là số người ban đầu không nhận được đồ uống chính xác của mình. Đó chính xác là những người tham gia vào quá trình trao đổi. Trong mỗi vòng đấu, bất kể lịch sử, tập hợp còn lại được xáo trộn đồng đều một cách ngẫu nhiên nên số người dự kiến ​​rời đi trong một vòng luôn là 1 miễn là còn lại ít nhất một người. Theo tuyến tính của kỳ vọng, tổng số vòng dự kiến ​​chính xác là k. Vì k không thể vượt quá n nên giá trị cực đại đạt được khi k = n, điều này xảy ra chính xác khi hoán vị là một biến đổi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input().strip())

if n == 1:
    print("0.0")
    print(1)
    exit()

# build derangement: shift by 1
p = list(range(2, n + 1)) + [1]

print(float(n))
print(*p)
```Bước đầu tiên xử lý trường hợp suy biến trong đó không thể thực hiện được quá trình trao đổi. Đối với tất cả n lớn hơn, việc xây dựng đảm bảo một hoán vị không có điểm cố định bằng cách sử dụng phép dịch chuyển tuần hoàn đơn giản. Điều này là đủ vì bất kỳ sự xáo trộn nào cũng là tối ưu. 

Giá trị được in chỉ đơn giản là được chuyển đổi sang định dạng dấu phẩy động. Không có sự phụ thuộc vào cơ cấu ngoài việc đảm bảo sự tham gia ban đầu tối đa. 

Một lỗi phổ biến ở đây là cố gắng mô phỏng quá trình hoặc tính toán xác suất một cách rõ ràng. Điều quan trọng là quá trình “đặt lại tính ngẫu nhiên” mỗi vòng, khiến hệ thống chỉ phụ thuộc vào kích thước được đặt hiện tại chứ không phụ thuộc vào lịch sử. 

## Ví dụ đã hoạt động 

Xét n = 3 với hoán vị [2, 3, 1]. 

| Vòng | Kích thước cài đặt hoạt động | Dự kiến ​​loại bỏ | Còn lại | 
| --- | --- | --- | --- | 
| 0 | 3 | - | 3 | 
| 1 | 3 | 1 | 2 | 
| 2 | 2 | 1 | 1 | 
| 3 | 1 | 1 | 0 | 

Dấu vết này cho thấy rằng mỗi vòng sẽ loại bỏ một người tham gia dự kiến ​​cho đến khi hệ thống trống. Kích thước ban đầu xác định trực tiếp số vòng dự kiến. 

Với n = 1 với hoán vị [1], hệ thống bắt đầu trống: 

| Vòng | Kích thước cài đặt hoạt động | Dự kiến ​​loại bỏ | Còn lại | 
| --- | --- | --- | --- | 
| 0 | 0 | - | 0 | 

Điều này xác nhận rằng không có sự xáo trộn nào xảy ra và số vòng dự kiến ​​là bằng không. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | dựng một hoán vị tuần hoàn đơn giản | 
| Không gian | O(n) | lưu trữ hoán vị | 

Các ràng buộc cho phép tối đa 1000 phần tử, do đó, việc xây dựng tuyến tính là đủ dễ dàng. Không có quá trình mô phỏng hoặc lặp lại nào được yêu cầu ngoài việc xây dựng hoán vị đầu ra. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(sys.stdin.readline().strip())

    if n == 1:
        return "0.0\n1\n"

    p = list(range(2, n + 1)) + [1]
    return f"{float(n)}\n" + " ".join(map(str, p)) + "\n"

# provided sample
assert run("2\n") == "2.0\n2 1\n"

# custom cases
assert run("1\n") == "0.0\n1\n", "minimum case"

assert run("3\n") == "3.0\n2 3 1\n", "small derangement"

assert run("5\n") == "5.0\n2 3 4 5 1\n", "cyclic shift correctness"

assert run("1000\n").split("\n")[0] == "1000.0\n".strip(), "large n format check"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 0,0 / 1 | trường hợp cơ sở đúng đắn | 
| 3 | 3.0 / 2 3 1 | hành vi chu kỳ nhỏ | 
| 5 | 5,0 / 2 3 4 5 1 | xây dựng loạn chung | 
| 1000 | 1000.0/hoán vị | khả năng mở rộng và định dạng | 

## Vỏ cạnh 

Với n = 1, thuật toán đưa ra một hoán vị tầm thường và trả về chính xác 0,0 vì không có người tham gia nào tham gia quá trình trao đổi. Logic được xây dựng hoàn toàn bỏ qua bước làm xáo trộn. 

Với n = 2, hoán vị [2, 1] đảm bảo cả hai người tham gia ban đầu đều không chính xác. Sau một lần xáo trộn, cả hai đều được giải quyết đồng thời theo kỳ vọng, mang lại chính xác số vòng dự kiến ​​cho hai người tham gia. 

Đối với n lớn hơn, sự dịch chuyển theo chu kỳ đảm bảo không có điểm cố định, nghĩa là tất cả những người tham gia đều đóng góp vào quá trình ngay từ đầu. Số vòng dự kiến ​​sẽ tỷ lệ tuyến tính với n vì quá trình này sẽ loại bỏ chính xác một người tham gia mỗi vòng theo kỳ vọng.
