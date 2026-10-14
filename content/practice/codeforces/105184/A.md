---
title: "CF 105184A - Cập nhật"
description: "Chúng ta được cung cấp một chuỗi chỉ bao gồm các chữ cái tiếng Anh viết thường. Chúng ta được phép thực hiện một thao tác chọn hai chữ cái x và y, sau đó thay thế mọi lần xuất hiện của x trong toàn bộ chuỗi bằng y trong một lần quét toàn cục. Hoạt động này có thể được lặp đi lặp lại bao nhiêu lần."
date: "2026-06-27T04:24:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105184
codeforces_index: "A"
codeforces_contest_name: "The 8th Hebei Collegiate Programming Contest"
rating: 0
weight: 105184
solve_time_s: 45
verified: true
draft: false
---

[CF 105184A - Cập nhật](https://codeforces.com/problemset/problem/105184/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 45s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi chỉ bao gồm các chữ cái tiếng Anh viết thường. Chúng ta được phép thực hiện thao tác chọn hai chữ cái`x`Và`y`, sau đó thay thế mọi lần xuất hiện của`x`trong toàn bộ chuỗi với`y`trong một cuộc càn quét toàn cầu. Hoạt động này có thể được lặp đi lặp lại bao nhiêu lần. 

Mục đích là chuyển đổi toàn bộ chuỗi sao cho mọi ký tự đều trở thành chữ cái`'i'`và chúng tôi muốn giảm thiểu số lượng thao tác cần thiết. 

Khía cạnh quan trọng là mỗi thao tác không mang tính cục bộ, nó ảnh hưởng đồng thời đến tất cả các lần xuất hiện của ký tự được chọn. Điều này đặt ra vấn đề làm thế nào các chữ cái có thể được “hợp nhất” dần dần vào các chữ cái khác cho đến khi mọi thứ sụp đổ thành một khối.`'i'`. 

Ràng buộc`|s| ≤ 10^4`có nghĩa là chúng tôi không cần phải mô phỏng bất cứ điều gì cho mỗi ký tự cho mỗi thao tác. Bất kỳ giải pháp nào liên tục quét hoặc mô phỏng các phép biến đổi trên chuỗi đầy đủ trong nhiều bước vẫn sẽ chỉ đạt nếu số lượng thao tác rất nhỏ và bị giới hạn bởi kích thước bảng chữ cái. Vì chỉ có 26 chữ cái có thể, nên chúng ta mong đợi một giải pháp phụ thuộc vào loại chữ cái hơn là độ dài chuỗi. 

Một điểm tinh tế là chúng ta không bị buộc phải sử dụng trực tiếp`'i'`như một biểu tượng trung gian trong quá trình hoạt động. Kể cả nếu`'i'`ban đầu không có trong chuỗi, chúng ta được phép ánh xạ bất kỳ chữ cái nào vào`'i'`. 

Các trường hợp cạnh đáng được tách biệt đến từ các bảng chữ cái nhỏ: 

Nếu chuỗi là`"i"`, câu trả lời là`0`vì không có gì thay đổi là cần thiết. 

Nếu chuỗi là`"abc"`, hành vi tối ưu là ánh xạ từng`a`,`b`, Và`c`vào trong`'i'`, thực hiện 3 thao tác. Một ý tưởng bất cẩn trước tiên có thể cố gắng sắp xếp mọi thứ vào một chữ cái và sau đó vào`'i'`, nhưng điều đó không bao giờ cải thiện số lượng. 

Nếu chuỗi là`"aii"`, chỉ một`a`cần phải thay đổi, vì vậy câu trả lời là`1`. 

Những ví dụ này cho thấy chi phí chỉ phụ thuộc vào số lượng chữ cái khác nhau`'i'`. 

## Phương pháp tiếp cận 

Cách giải thích brute-force là nghĩ đến việc áp dụng các thao tác từng bước, duy trì toàn bộ chuỗi mỗi lần. Ở mỗi bước, chúng ta có thể thử tất cả các cặp`(x, y)`và mô phỏng thay thế`x`với`y`, sau đó lặp lại hoặc tìm kiếm số bước tối thiểu để đạt đến trạng thái có tất cả các ký tự`'i'`. Đây là bài toán đường đi ngắn nhất trên tất cả các chuỗi có thể, trong đó mỗi chuyển đổi trạng thái là một sự thay thế toàn cục. 

Không gian trạng thái lớn về mặt thiên văn. Mặc dù mỗi chuỗi có độ dài lên tới`10^4`, số chuỗi phân biệt có thể là`26^n`và ngay cả khi chúng ta bỏ qua cấu trúc và chỉ nghĩ đến các phân vùng chữ cái, các chuyển đổi vẫn tạo thành một biểu đồ trên các trạng thái hàm mũ. Điều này ngay lập tức làm cho vũ lực không thể thực hiện được. 

Quan sát quan trọng là thao tác này chỉ ảnh hưởng đến nhận dạng chữ cái chứ không ảnh hưởng đến vị trí. Mọi vị trí chứa cùng một chữ cái sẽ hoạt động giống hệt nhau mãi mãi. Vì vậy, trạng thái được mô tả đầy đủ bằng các chữ cái hiện tồn tại trong chuỗi. 

Từ quan điểm này, mỗi thao tác sẽ loại bỏ một ký hiệu chữ cái`x`và thay thế nó bằng`y`. Điều này làm giảm nghiêm trọng số lượng các chữ cái riêng biệt trừ khi`x`Và`y`đã giống nhau rồi. Vì chúng ta muốn mọi thứ trở thành`'i'`, các chữ cái duy nhất quan trọng là các chữ cái riêng biệt trong chuỗi gốc ngoài`'i'`. Mỗi chữ cái như vậy phải bị “loại bỏ” ít nhất một lần, vì không gì khác có thể làm nó biến mất ngoại trừ việc ánh xạ nó vào một biểu tượng khác, và cuối cùng mọi thứ đều phải sụp đổ thành một biểu tượng khác.`'i'`. 

Chúng ta luôn có thể thực hiện điều này một cách tối ưu bằng cách ánh xạ trực tiếp từng`'i'`thư vào`'i'`một lần. Bất kỳ chuỗi gián tiếp nào như`a -> b -> i`không làm giảm số lượng hoạt động vì`a -> b`không giúp giảm số lượng các chữ cái riêng biệt trừ khi`b`đã rồi`'i'`. 

Vì vậy, câu trả lời hoàn toàn mang tính tổ hợp: đếm xem có bao nhiêu chữ cái riêng biệt xuất hiện trong chuỗi và trừ đi một chữ cái nếu`'i'`nằm trong số đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên các trạng thái chuỗi | O(26! hoặc hàm mũ) | O(tiểu bang) | Quá chậm | 
| Đếm chữ cái riêng biệt | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giảm bớt vấn đề để theo dõi những chữ cái tồn tại trong chuỗi. 

1. Quét chuỗi một lần và ghi lại những chữ cái xuất hiện. 

Điều này quan trọng vì chỉ có tập hợp các ký tự riêng biệt mới ảnh hưởng đến số lượng thao tác; các vị trí không liên quan. 
2. Đếm tổng cộng có bao nhiêu chữ cái riêng biệt. 
3. Kiểm tra xem`'i'`nằm trong số những bức thư này. 
4. Nếu`'i'`có mặt, câu trả lời là`distinct_count - 1`, từ`'i'`không cần phải loại bỏ hoặc chuyển đổi. 
5. Nếu`'i'`không có mặt, câu trả lời là`distinct_count`, vì mỗi chữ cái phải được chuyển trực tiếp thành`'i'`. 

Quyết định ở bước 4 và 5 phản ánh liệu`'i'`là một phần của bảng chữ cái đầu tiên của chuỗi hoặc phải được đưa vào thông qua một phép biến đổi. 

### Tại sao nó hoạt động 

Mỗi thao tác loại bỏ chính xác một chữ cái nguồn`x`bằng cách hợp nhất nó vào một số chữ cái đích`y`. Quá trình tiếp tục cho đến khi chỉ`'i'`vẫn còn. Không có thao tác nào có thể loại bỏ nhiều hơn một chữ cái riêng biệt khỏi hệ thống vì chỉ có một`x`được chọn cho mỗi hoạt động. Vì vậy, mỗi chữ cái riêng biệt khác với`'i'`cần ít nhất một thao tác. Một sự chuyển đổi trực tiếp từ mỗi chữ cái như vậy thành`'i'`đạt được giới hạn dưới này, làm cho nó trở nên tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s = input().strip()
    
    present = [False] * 26
    for ch in s:
        present[ord(ch) - ord('a')] = True

    distinct_count = sum(present)
    i_idx = ord('i') - ord('a')

    if present[i_idx]:
        print(distinct_count - 1)
    else:
        print(distinct_count)

if __name__ == "__main__":
    solve()
```Việc triển khai chỉ theo dõi sự hiện diện của các chữ cái, tránh mọi mô phỏng thay thế. Sự tinh tế quan trọng là đảm bảo chúng ta xử lý`'i'`đặc biệt khi nó tồn tại, vì nó không cần phải chuyển hóa đi. 

## Ví dụ đã hoạt động 

### Ví dụ 1:`s = "abc"`| Bước | Chữ cái riêng biệt | Chứa 'tôi' | Trả lời | 
| --- | --- | --- | --- | 
| Ban đầu | {a, b, c} | Không | 3 | 

Từ`'i'`không có mặt, mỗi chữ cái phải được chuyển đổi thành`'i'`. Mỗi yêu cầu một thao tác, vì vậy kết quả là 3. 

Điều này xác nhận rằng các phép biến đổi gián tiếp như`a -> b -> c -> i`không thể giảm số lượng vì mỗi lần loại bỏ vẫn tiêu tốn một chữ cái. 

### Ví dụ 2:`s = "aii"`| Bước | Chữ cái riêng biệt | Chứa 'tôi' | Trả lời | 
| --- | --- | --- | --- | 
| Ban đầu | {a, tôi} | Có | 1 | 

Chỉ một`a`cần phải được chuyển hóa. Một thao tác`a -> i`đủ. 

Điều này cho thấy rằng việc lặp đi lặp lại của một chữ cái không thành vấn đề, chỉ có các loại riêng biệt mới làm được điều đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chuyển một lần qua chuỗi để đánh dấu các chữ cái riêng biệt | 
| Không gian | O(1) | Đã sửa mảng kích thước 26 để theo dõi bảng chữ cái | 

Các ràng buộc cho phép tối đa 10^4 ký tự, do đó, quét tuyến tính là đủ dễ dàng. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    s = input().strip()
    present = [False] * 26
    for ch in s:
        present[ord(ch) - ord('a')] = True

    distinct_count = sum(present)
    i_idx = ord('i') - ord('a')

    if present[i_idx]:
        return str(distinct_count - 1)
    else:
        return str(distinct_count)

# minimal cases
assert run("i\n") == "0"
assert run("a\n") == "1"

# all same non-i
assert run("aaaa\n") == "1"

# includes i
assert run("ai\n") == "1"

# no i present, multiple letters
assert run("abc\n") == "3"

# large mixed
assert run("abcdefghijklmnopqrstuvwxyz\n") == "25"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`"i"`|`0`| Đã là nhân vật mục tiêu | 
|`"aaaa"`|`1`| Thư không phải mục tiêu duy nhất | 
|`"ai"`|`1`| Xử lý sự hiện diện của`'i'`| 
|`"abc"`|`3`| Tất cả các chữ cái cần chuyển đổi | 
| bảng chữ cái đầy đủ |`25`| Trường hợp khác biệt tối đa | 

## Vỏ cạnh 

Nếu chuỗi chỉ chứa`'i'`, thuật toán chỉ đánh dấu một chữ cái riêng biệt và trừ đi một chữ cái vì`'i'`đang có mặt. Giá trị được tính toán trở thành 0, phù hợp với thực tế là không cần thực hiện thao tác nào. 

Đối với một chuỗi như`"zzzz"`, tập riêng biệt chỉ chứa`'z'`, và kể từ đó`'i'`vắng mặt, câu trả lời là một. Thuật toán thực hiện chính xác một phép biến đổi khái niệm duy nhất`z -> i`. 

Đối với một chuỗi chứa mọi chữ cái bao gồm`'i'`, chẳng hạn như`"abci"`, số đếm riêng biệt là 4 và`'i'`có mặt nên câu trả lời là 3. Thuật toán tránh tính đúng`'i'`như một lá thư cần loại bỏ.
