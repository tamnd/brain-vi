---
title: "CF 104699C - \u0411\u0430\u0440\u0431\u0438 \u0432 \u0440\u0435\u0430\u043b\u044c\u043d\u043e\u043c \u043c\u0438\u0440\u0435"
description: "Chúng ta được cho một dãy kệ, mỗi kệ chứa một số lượng búp bê cố định. Ban đầu, một nhóm trẻ em được phân bổ trên các kệ này và mỗi giây, mỗi đứa trẻ đứng ở kệ sẽ lấy một con búp bê từ kệ đó."
date: "2026-06-29T08:32:37+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104699
codeforces_index: "C"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2023-2024, \u0412\u0442\u043e\u0440\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 104699
solve_time_s: 74
verified: true
draft: false
---

[CF 104699C - \u0411\u0430\u0440\u0431\u0438 \u0432 \u0440\u0435\u0430\u043b\u044c\u043d\u043e\u043c \u043c\u0438\u0440\u0435](https://codeforces.com/problemset/problem/104699/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 14s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy kệ, mỗi kệ chứa một số lượng búp bê cố định. Ban đầu, một nhóm trẻ em được phân bổ trên các kệ này và mỗi giây, mỗi đứa trẻ đứng ở kệ sẽ lấy một con búp bê từ kệ đó. Nếu một kệ hết búp bê trong một giây, chỉ một số trẻ có thể lấy búp bê từ đó thành công và những trẻ còn lại ngay lập tức được chuyển đến các kệ khác vẫn còn đủ búp bê để hỗ trợ chúng trong giây tiếp theo. Một số trẻ cuối cùng có thể không tìm thấy bất kỳ kệ hợp lệ nào để tiếp tục và rời đi với bất cứ thứ gì chúng đã thu thập được. 

Điểm tự do chính của bài toán này là chúng ta được phép chọn vị trí ban đầu của tất cả trẻ em trên các kệ. Câu hỏi đặt ra là liệu có tồn tại một số nhiệm vụ ban đầu sao cho cuối cùng mọi đứa trẻ đều nhận được chính xác số búp bê như nhau hay không. 

Dữ liệu đầu vào cho biết số lượng kệ và trẻ em, tiếp theo là số lượng búp bê trên mỗi kệ. Đầu ra là một quyết định nhị phân: liệu sự phân công công bằng như vậy có tồn tại hay không. 

Ràng buộc$n \le 10^5$Và$m \le 10^9$ngay lập tức cho chúng ta biết rằng bất kỳ mô phỏng nào ở cấp độ từng trẻ em hoặc từng giây đều không thể thực hiện được. Ngay cả việc lặp lại tất cả trẻ em cũng đã là quá lớn và việc mô phỏng chuyển động của chúng trên các kệ rõ ràng sẽ vượt quá giới hạn. Giải pháp phải thu gọn toàn bộ quá trình thành một số lượng nhỏ số lượng tổng hợp có nguồn gốc từ mảng. 

Trường hợp phức tạp xuất hiện khi tổng số búp bê nhỏ hơn số trẻ em. Trong trường hợp đó, ngay cả khi chúng tôi phân phối hoàn hảo thì ít nhất một đứa trẻ cũng không thể nhận được dù chỉ một con búp bê, khiến việc phân phối đồng đều là không thể. Một trường hợp khác phát sinh khi tổng số búp bê không chia hết cho số trẻ em. Vì mọi con búp bê cuối cùng đều bị một đứa trẻ nào đó lấy đi và chúng ta yêu cầu số lượng cuối cùng bằng nhau nên việc chia hết là cần thiết. 

Một cách tiếp cận ngây thơ có thể cố gắng gán trẻ một cách tham lam vào các kệ và mô phỏng dòng chảy, nhưng điều này nhanh chóng trở nên mơ hồ vì trẻ có thể di chuyển linh hoạt giữa các kệ và sự tương tác của chúng phụ thuộc vào sự tiến hóa theo thời gian thay vì phân bổ tĩnh. Điều này làm cho việc suy luận trực tiếp về các đường dẫn riêng lẻ trở nên sai lệch. 

## Phương pháp tiếp cận 

Một chiến lược mạnh mẽ sẽ là mô phỏng rõ ràng toàn bộ quá trình: xếp trẻ vào kệ, chạy quy trình tiêu thụ từng giây, xử lý các chuyển động tràn và theo dõi số lượng búp bê mà mỗi trẻ thu thập được. Điều này dễ hiểu về mặt khái niệm vì nó tuân theo lời tuyên bố theo nghĩa đen. Tuy nhiên, mỗi giây có thể liên quan đến tất cả trẻ em ở một kệ và trẻ em có thể di chuyển liên tục qua các kệ. Trong trường hợp xấu nhất, điều này tạo ra theo thứ tự$O(m \cdot \text{time})$hoạt động, điều này là không thể thực hiện được vì$m$bản thân nó có thể lên tới$10^9$. 

Quan sát quan trọng là bất chấp các quy tắc chuyển động phức tạp, không có con búp bê nào được tạo ra hoặc phá hủy, và mỗi con búp bê cuối cùng sẽ bị chính xác một đứa trẻ lấy đi cho đến khi quá trình kết thúc. Động lực của chuyển động chỉ ảnh hưởng đến _ai_ lấy con búp bê nào chứ không ảnh hưởng đến _tổng cộng bao nhiêu con búp bê được lấy đi_. Vì vậy, số lượng có ý nghĩa duy nhất là tổng số búp bê có sẵn. 

Nếu tổng số búp bê là$S = \sum a_i$, thì trong bất kỳ kịch bản thành công nào, tất cả búp bê đều phải được chia đều cho$m$trẻ em, vì mọi đứa trẻ đều có kết thúc bằng cùng một số. Điều này ngay lập tức ngụ ý rằng mỗi đứa trẻ phải nhận được$S / m$búp bê, phải là số nguyên. 

Một khi điều kiện này được giữ vững thì không có ràng buộc cấu trúc bổ sung nào được áp đặt bởi các quy tắc chuyển động. Bất kỳ sự sắp xếp nào cũng có thể được hiểu là một luồng các đơn vị giống hệt nhau và vì tất cả búp bê đều tương đương nhau nên chúng ta luôn có thể chỉ định trẻ em theo cách thực hiện được sự chia đều. 

Do đó, vấn đề hoàn toàn quy về việc kiểm tra xem tổng số tiền có chia hết cho số con hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(quy trình động lớn) | O(m) | Quá chậm | 
| Kiểm tra tổng và chia hết | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng số búp bê trên tất cả các kệ. Điều này thể hiện toàn bộ nguồn lực cần được phân bổ cho tất cả trẻ em. 
2. Kiểm tra xem tổng số này ít nhất có lớn bằng số trẻ em hay không. Nếu nó nhỏ hơn, một số trẻ nhất thiết sẽ không nhận được búp bê nào trong khi những trẻ khác nhận được nhiều hơn, khiến sự bình đẳng là không thể. 
3. Kiểm tra xem tổng số búp bê có chia hết cho số trẻ em không. Nếu nó không chia hết được thì không có sự phân chia bằng nhau bất kể các phần tử con được sắp xếp như thế nào. 
4. Nếu cả hai điều kiện đều được thỏa mãn thì kết luận rằng có một thỏa thuận hợp lệ. 

Lý do đằng sau thủ tục này là quá trình này hoàn toàn mang tính phân phối lại. Phong trào trẻ em chỉ ảnh hưởng đến trật tự tiêu dùng ở địa phương chứ không ảnh hưởng đến tổng thể toàn cầu. Vì mỗi con búp bê được tiêu thụ đúng một lần nên việc phân phối cuối cùng buộc phải tôn trọng việc bảo tồn toàn cầu. 

### Tại sao nó hoạt động 

Hệ thống luôn kết thúc với mỗi con búp bê được tiêu thụ được giao cho chính xác một đứa trẻ và không đứa trẻ nào có thể kết thúc với nhiều búp bê hơn mức tồn tại trong tổng số phân phối cho phép. Bởi vì tất cả trẻ em đều không thể phân biệt được trong yêu cầu cuối cùng, nên cấu hình ổn định duy nhất có thể có là cấu hình trong đó tổng số búp bê được tiêu thụ được phân chia đều. Bất kỳ sự mất cân bằng nào cũng sẽ hàm ý sự phân bổ theo tỷ lệ hoặc phần còn lại, điều này mâu thuẫn với bản chất rời rạc của tiêu dùng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    
    total = sum(a)
    
    if total < m:
        print("NO")
        return
    
    if total % m != 0:
        print("NO")
        return
    
    print("YES")

if __name__ == "__main__":
    solve()
```Giải pháp đọc mảng, tổng hợp tổng số búp bê và áp dụng hai điều kiện cần thiết bắt nguồn từ bảo tồn toàn cầu. Không cần theo dõi vị trí hoặc mô phỏng chuyển động vì quá trình này không ảnh hưởng đến tính khả thi tổng thể, chỉ ảnh hưởng đến thứ tự phân phối. 

Một sai lầm phổ biến là cố gắng làm mẫu chuyển động của trẻ giữa các kệ. Điều đó gây ra sự phức tạp không cần thiết mà không làm thay đổi tiêu chí kết quả. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3 3
3 4 5
```Tổng số búp bê là 12 và có 3 đứa trẻ. Mỗi đứa trẻ sẽ cần nhận được 4 con búp bê. 

| Bước | Tổng số búp bê | Trẻ em | Kiểm tra | 
| --- | --- | --- | --- | 
| 1 | 12 | 3 | 12 ≥ 3 | 
| 2 | 12 | 3 | 12 % 3 = 0 | 

Vì cả hai điều kiện đều đạt nên câu trả lời là CÓ. 

Điều này chứng tỏ trường hợp có thể phân phối lại mặc dù các kệ có sự khác biệt đáng kể, bởi vì chỉ có tổng hợp mới quan trọng. 

### Mẫu 2 

đầu vào:```
6 3
2 3 3 5 1 3
```Tổng số búp bê là 17 con, có 3 con. Mỗi đứa trẻ sẽ cần$17/3$, không phải là số nguyên. 

| Bước | Tổng số búp bê | Trẻ em | Kiểm tra | 
| --- | --- | --- | --- | 
| 1 | 17 | 3 | 17 ≥ 3 | 
| 2 | 17 | 3 | 17 % 3 ≠ 0 | 

Vì khả năng chia hết không thành công nên câu trả lời là KHÔNG. 

Điều này cho thấy một tình huống mặc dù có đủ số lượng búp bê về tổng thể nhưng việc phân phối đồng đều về mặt cấu trúc là không thể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chúng tôi chỉ tính một tổng duy nhất trên các kệ | 
| Không gian | O(1) | Chỉ có một số biến vô hướng được sử dụng | 

Giải pháp dễ dàng phù hợp với các ràng buộc vì$n \le 10^5$và một lần truyền tuyến tính duy nhất trên mảng là không đáng kể về thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from math import *
    
    n, m = map(int, sys.stdin.readline().split())
    a = list(map(int, sys.stdin.readline().split()))
    
    total = sum(a)
    if total < m or total % m != 0:
        return "NO"
    return "YES"

# provided samples
assert run("3 3\n3 4 5\n") == "YES"
assert run("6 3\n2 3 3 5 1 3\n") == "NO"

# custom cases
assert run("1 1\n10\n") == "YES", "single child trivial"
assert run("1 5\n4\n") == "NO", "insufficient total"
assert run("4 2\n1 1 1 1\n") == "YES", "even split possible"
assert run("5 3\n1 2 3 4 5\n") == "NO", "non divisible sum"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1/10 | CÓ | trường hợp một con tầm thường | 
| 1 5/4 | KHÔNG | tổng nguồn lực không đủ | 
| 4 2 / 1 1 1 1 | CÓ | phân vùng sạch thậm chí | 
| 5 3 / 1 2 3 4 5 | KHÔNG | lỗi chia hết | 

## Vỏ cạnh 

Một trường hợp tối thiểu xảy ra khi chỉ có một đứa trẻ. Trong tình huống đó, bất kỳ tổng số khác 0 nào cũng có tác dụng ngay lập tức vì tất cả búp bê đều thuộc về đứa trẻ đó một cách tự nhiên và tính chia hết luôn được giữ nguyên. 

Khi tổng số búp bê nhỏ hơn số trẻ em, quy trình không thể chỉ định dù chỉ một con búp bê cho mỗi trẻ, do đó sự bình đẳng là không thể. Thuật toán từ chối chính xác điều này thông qua`total < m`kiểm tra. 

Khi tổng chia hết cho số trẻ em, thì ngay cả việc phân bổ không đồng đều trên các kệ cũng không thành vấn đề. Hệ thống luôn có thể được hiểu là phân phối lại các đơn vị giống hệt nhau, do đó tính khả thi chỉ phụ thuộc vào tính nhất quán số học mà thuật toán nắm bắt trực tiếp.
