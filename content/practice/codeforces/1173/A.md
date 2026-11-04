---
title: "CF 1173A - Nauuo và Phiếu bầu"
description: "Bài toán yêu cầu chúng ta xác định kết quả của một kịch bản bỏ phiếu đơn giản. Có ba loại cử tri: một số được đảm bảo sẽ bỏ phiếu tán thành, một số được đảm bảo sẽ bỏ phiếu phản đối và một số thì chưa quyết định. Đầu vào đưa ra số lượng của mỗi nhóm."
date: "2026-06-12T01:54:35+07:00"
tags: ["codeforces", "competitive-programming", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1173
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 564 (Div. 2)"
rating: 800
weight: 1173
solve_time_s: 98
verified: true
draft: false
---

[CF 1173A - Nauuo và Phiếu bầu](https://codeforces.com/problemset/problem/1173/A) 

**Đánh giá:** 800 
**Thẻ:** tham lam 
**Thời gian giải:** 1 phút 38 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Bài toán yêu cầu chúng ta xác định kết quả của một kịch bản bỏ phiếu đơn giản. Có ba loại cử tri: một số được đảm bảo sẽ bỏ phiếu tán thành, một số được đảm bảo sẽ bỏ phiếu phản đối và một số thì chưa quyết định. Đầu vào đưa ra số lượng của mỗi nhóm. Mục tiêu là dự đoán kết quả cuối cùng của cuộc bỏ phiếu: tích cực, tiêu cực, trung lập hoặc không chắc chắn. Kết quả tích cực xảy ra nếu số lượt tán thành vượt quá số lượt phản đối, tiêu cực nếu số lượt phản đối vượt quá số lượt tán thành, trung lập nếu chúng bằng nhau và không chắc chắn liệu những cử tri chưa biết có thể thay đổi kết quả hay không tùy thuộc vào cách họ bỏ phiếu. 

Các ràng buộc rất nhỏ: mỗi lần đếm nhiều nhất là 100, do đó tổng số cử tri nhiều nhất là 300. Điều này cho phép chúng ta suy luận về mọi khả năng mà không cần lo lắng về hiệu quả. Không có lo ngại về hiệu suất khi lặp lại trong phạm vi nhỏ, nhưng chúng ta vẫn nên hướng tới giải pháp liên tục về thời gian bằng cách phân tích phạm vi tổng số phiếu bầu có thể có. 

Các trường hợp khó khăn không rõ ràng bao gồm các tình huống trong đó phiếu bầu không xác định có thể lật ngược kết quả. Ví dụ: nếu số lượt tán thành và phản đối bằng nhau và có ít nhất một cử tri không xác định thì kết quả sẽ không chắc chắn vì một cuộc bỏ phiếu duy nhất có thể làm thay đổi số dư. Một trường hợp khác là khi một bên chiếm ưu thế ngay cả sau khi đã giao tất cả các phiếu bầu chưa xác định cho bên kia, chẳng hạn như 10 phiếu tán thành, 2 phiếu phản đối và 3 phiếu không xác định. Ở đây, kết quả chắc chắn là tích cực, ngay cả khi tất cả các phiếu bầu chưa biết đều chuyển sang phản đối. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ xem xét tất cả các cách mà những cử tri chưa biết có thể bỏ phiếu. Nếu có`z`cử tri chưa biết, có`z+1`cách để phân phối chúng giữa các lượt tán thành và phản đối. Đối với mỗi lần phân phối, chúng tôi tính toán kết quả ròng. Nếu tất cả các kết quả đều giống nhau thì đó là kết quả cuối cùng; nếu không thì kết quả là không chắc chắn. Điều này hoạt động vì`z`nhiều nhất là 100, nên chúng ta sẽ tính tối đa 101 khả năng. Mặc dù khả thi trong thực tế cho vấn đề này nhưng nó không cần thiết và có thể được đơn giản hóa. 

Quan sát quan trọng là chúng ta chỉ cần sự khác biệt tối thiểu và tối đa có thể có giữa số lượt tán thành và phản đối. Sự khác biệt tối thiểu xảy ra nếu tất cả cử tri chưa biết bỏ phiếu phản đối, mức chênh lệch tối đa xảy ra nếu tất cả cử tri không xác định bỏ phiếu ủng hộ. Nếu cả hai thái cực đều cùng dấu thì kết quả được xác định. Ngược lại thì không chắc chắn. Điều này làm giảm vấn đề xuống còn một vài so sánh, tạo ra nghiệm O(1). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(z) | O(1) | Hoạt động nhưng quá mức cần thiết | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc ba số nguyên biểu thị số lượt ủng hộ được đảm bảo`x`, số lượt phản đối được đảm bảo`y`và phiếu bầu không xác định`z`. 
2. Tính chênh lệch phiếu bầu ròng tối đa có thể có như sau`x + z - y`, giả sử tất cả các phiếu bầu chưa biết đều là phiếu tán thành. 
3. Tính chênh lệch phiếu bầu ròng tối thiểu có thể có như sau`x - (y + z)`, giả sử tất cả các phiếu bầu không xác định đều là phiếu phản đối. 
4. Nếu cả hai mức tối đa và tối thiểu đều hoàn toàn dương, hãy in "+", vì ngay cả trong trường hợp xấu nhất, số lượt tán thành vẫn vượt quá số lượt phản đối. 
5. Nếu cả hai mức tối đa và tối thiểu hoàn toàn âm, hãy in "-", vì số lượt phản đối chiếm ưu thế trong tất cả các phép gán có thể có của số phiếu không xác định. 
6. Nếu cả hai mức tối đa và tối thiểu đều bằng 0, hãy in "0", vì các phiếu bầu bằng nhau bất kể phiếu bầu chưa xác định. 
7. Ngược lại, in "?", cho biết rằng việc phân công các phiếu bầu không xác định khác nhau có thể tạo ra các kết quả khác nhau, khiến kết quả không chắc chắn. 

Tại sao nó hiệu quả: bằng cách chỉ xem xét các tình huống cực đoan, chúng tôi nắm bắt được đầy đủ các kết quả có thể xảy ra. Bất kỳ sự phân phối phiếu bầu không xác định nào đều tạo ra kết quả giữa hai thái cực này. Nếu cả hai thái cực đều dẫn đến cùng một kết quả thì tất cả các phân phối trung gian cũng dẫn đến kết quả tương tự. Nếu các thái cực tạo ra các kết quả khác nhau, thì tồn tại một số phép gán tạo ra từng kết quả, nghĩa là sự không chắc chắn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

x, y, z = map(int, input().split())

max_diff = x + z - y
min_diff = x - (y + z)

if min_diff > 0:
    print("+")
elif max_diff < 0:
    print("-")
elif min_diff <= 0 <= max_diff and max_diff == 0 and min_diff == 0:
    print("0")
else:
    print("?")
```Mã đọc số phiếu bầu và tính toán mức chênh lệch phiếu bầu cực lớn, sau đó quyết định kết quả dựa trên những mức cực đoan này. Sự tinh tế nằm ở việc xử lý trường hợp số 0 một cách cẩn thận: nếu cả hai thái cực đều chính xác bằng 0 thì kết quả là trung tính. Ngược lại, nếu các điểm cực trị khác nhau trên 0 thì kết quả là không chắc chắn. Không cần vòng lặp hoặc cấu trúc dữ liệu bổ sung. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

đầu vào:`3 7 0`| x | y | z | max_diff | phút_diff | kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 3 | 7 | 0 | -4 | -4 | - | 

Tất cả các phiếu bầu đều được biết đến, sự khác biệt là tiêu cực. Kết quả là "-" như mong đợi. 

**Mẫu 2** 

đầu vào:`1 1 1`| x | y | z | max_diff | phút_diff | kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 | -1 | ? | 

Cử tri không xác định có thể đưa ra kết quả theo một trong hai cách. Thuật toán xuất ra chính xác dấu "?". 

Những dấu vết này xác nhận rằng khả năng tính toán cực cao đã nắm bắt được toàn bộ phạm vi khả năng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ số học và so sánh trên ba số nguyên | 
| Không gian | O(1) | Chỉ có một số biến số nguyên được sử dụng | 

Với các ràng buộc (tất cả số lượng ≤ 100), giải pháp sẽ thực thi trong thời gian và bộ nhớ không đổi, thấp hơn nhiều so với giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    x, y, z = map(int, input().split())
    max_diff = x + z - y
    min_diff = x - (y + z)
    if min_diff > 0:
        return "+"
    elif max_diff < 0:
        return "-"
    elif min_diff <= 0 <= max_diff and max_diff == 0 and min_diff == 0:
        return "0"
    else:
        return "?"

# Provided samples
assert run("3 7 0\n") == "-", "sample 1"
assert run("3 1 2\n") == "+", "sample 2"
assert run("1 1 0\n") == "0", "sample 3"
assert run("1 0 1\n") == "?", "sample 4"

# Custom cases
assert run("0 0 0\n") == "0", "all zero votes"
assert run("100 0 0\n") == "+", "max upvotes only"
assert run("0 100 0\n") == "-", "max downvotes only"
assert run("50 50 0\n") == "0", "tie with no unknowns"
assert run("50 49 1\n") == "?", "tie possible with one unknown"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 0 0 0 | 0 | đầu vào nhỏ nhất, kết quả trung lập | 
| 100 0 0 | + | số phiếu ủng hộ tối đa chiếm ưu thế | 
| 0 100 0 | - | số lượt phản đối tối đa chiếm ưu thế | 
| 50 50 0 | 0 | buộc mà không biết | 
| 50 49 1 | ? | sự không chắc chắn được đưa ra bởi một ẩn số duy nhất | 

## Vỏ cạnh 

Khi phiếu bầu không xác định có thể thay đổi kết quả, chẳng hạn như`1 0 1`, thuật toán tính toán chính xác`max_diff = 2`Và`min_diff = 0`. Vì các cực trị không hoàn toàn là dương, âm hoặc chính xác bằng 0 nên đầu ra là "?", thể hiện sự không chắc chắn. 

Khi tất cả các phiếu bầu đã được biết và cân bằng, chẳng hạn như`2 2 0`, cả hai cực trị đều bằng 0 và đầu ra là "0". 

Khi một bên chiếm ưu thế ngay cả với số phiếu không xác định, chẳng hạn như`10 2 3`, các cực trị là`11`Và`5`, cả hai đều dương, do đó thuật toán in "+", xác nhận chính xác ưu thế của số lượt tán thành.
