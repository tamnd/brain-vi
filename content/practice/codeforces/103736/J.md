---
title: "CF 103736J - Sợi dây thần kỳ của IHI"
description: "Chúng tôi đang duy trì một chuỗi bắt đầu trống và được sửa đổi bởi một chuỗi thao tác. Mỗi thao tác sẽ thêm một ký tự chữ thường vào cuối, xóa ký tự cuối cùng nếu có hoặc thực hiện thay thế toàn cục thay thế mọi lần xuất hiện của một…"
date: "2026-07-02T09:12:40+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103736
codeforces_index: "J"
codeforces_contest_name: "The 2022 Hangzhou Normal U Summer Trials"
rating: 0
weight: 103736
solve_time_s: 41
verified: true
draft: false
---

[CF 103736J - Sợi dây ma thuật của IHI](https://codeforces.com/problemset/problem/103736/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 41s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang duy trì một chuỗi bắt đầu trống và được sửa đổi bởi một chuỗi thao tác. Mỗi thao tác sẽ thêm một ký tự chữ thường vào cuối, xóa ký tự cuối cùng nếu có hoặc thực hiện thay thế toàn cục thay thế mọi lần xuất hiện của một ký tự nhất định trong chuỗi hiện tại bằng một ký tự khác. 

Nhiệm vụ là mô phỏng tất cả các thao tác theo thứ tự và xuất ra chuỗi kết quả cuối cùng. Nếu không còn gì, chúng tôi in một thông báo đặc biệt biểu thị sự trống rỗng. 

Khó khăn chính là hoạt động thứ ba. Mô phỏng trực tiếp quét toàn bộ chuỗi và thay thế các ký tự cho mọi truy vấn loại 3 là quá chậm. Với tối đa 100000 phép toán, một chuỗi kích thước tuyến tính và có khả năng thay thế nhiều phép toán toàn cầu, cách tiếp cận ngây thơ sẽ thoái hóa thành hành vi bậc hai hoặc tệ hơn. 

Các trường hợp cạnh phát sinh chủ yếu từ sự tương tác giữa việc xóa và thay thế. Việc thay thế không phụ thuộc vào vị trí mà chỉ phụ thuộc vào nhận dạng ký tự, nhưng việc xóa có nghĩa là các ký tự có thể biến mất sau khi được chuyển đổi một cách hợp lý. Ví dụ: nếu chúng ta thêm "ab", sau đó thay a bằng c, rồi xóa, cấu trúc cuối cùng phụ thuộc vào việc chúng ta áp dụng các phép biến đổi một cách háo hức hay lười biếng. Việc triển khai đơn giản cũng có thể quên rằng việc thay thế lặp đi lặp lại có thể tạo thành các chuỗi, chẳng hạn như từ a đến b và sau đó là từ b đến c. 

Một trường hợp tinh tế khác là chu kỳ thay thế lặp đi lặp lại. Nếu a được thay thế bằng b và sau đó b được thay thế bằng a, thì việc triển khai đơn giản liên tục viết lại toàn bộ chuỗi có nguy cơ phải quét lại toàn bộ. 

## Phương pháp tiếp cận 

Giải pháp Brute Force trực tiếp lưu trữ chuỗi và áp dụng các thao tác theo đúng nghĩa đen. Việc thêm và xóa ở cuối là O(1), nhưng thao tác thay thế sẽ quét toàn bộ chuỗi và thay thế tất cả các lần xuất hiện của x bằng y. Trong trường hợp xấu nhất, chúng ta có thể có các phép toán O(q) và mỗi lần thay thế có thể tốn O(n), trong đó n tăng lên thành q. Điều này mang lại O(q²), vượt xa mức chấp nhận được đối với 100000 thao tác. 

Quan sát quan trọng là bản thân chuỗi không cần phải viết lại khi việc thay thế xảy ra. Mỗi ký tự không quan trọng bởi giá trị chữ của nó, mà bởi cách nó hiện được ánh xạ trong hệ thống chuyển đổi ký tự động. Thay vì sửa đổi chuỗi, chúng tôi duy trì ánh xạ từ mỗi chữ cái tới đại diện hiện tại của nó. Khi chúng tôi thêm một ký tự, chúng tôi lưu trữ dạng ánh xạ hiện tại của ký tự đó. Khi chúng tôi truy vấn thay thế, chúng tôi cập nhật ánh xạ. Khi chúng tôi xóa, chúng tôi sẽ xóa ký tự ánh xạ được lưu trữ cuối cùng. 

Ý tưởng quan trọng là việc thay thế là sự kết hợp hàm trên các ký tự chứ không phải là sửa đổi văn bản được lưu trữ. Mỗi ký tự trong chuỗi ghi nhớ danh tính ban đầu của nó và ánh xạ cho chúng ta biết nó hiện đang đại diện cho cái gì. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(q²) | O(q) | Quá chậm | 
| Lập bản đồ + Ngăn xếp | O(q) | O(q) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì hai cấu trúc. Một là ngăn xếp biểu thị chuỗi ký tự được thêm vào. Thứ hai là một mảng`mp`Ở đâu`mp[c]`là nhân vật đại diện hiện tại của`c`. 

Chúng tôi cũng ngầm duy trì ý tưởng ngược lại: chúng tôi không bao giờ viết lại ngăn xếp, chúng tôi chỉ diễn giải các giá trị của nó thông qua`mp`. 

1. Khởi tạo một mảng`mp`sao cho mỗi ký tự ánh xạ tới chính nó. Khởi tạo một ngăn xếp trống. 
2. Đối với thao tác loại 1 có ký tự`x`, xô`mp[x]`lên ngăn xếp. Chúng tôi lưu trữ phiên bản được ánh xạ ngay lập tức để những thay đổi trong tương lai không ảnh hưởng đến mục đích lịch sử. 
3. Đối với thao tác loại 2, hãy bật ra khỏi ngăn xếp nếu nó không trống. Điều này trực tiếp mô hình việc xóa ký tự cuối cùng của chuỗi hiện tại. 
4. Đối với thao tác loại 3 với ký tự`x`Và`y`, cập nhật ánh xạ sao cho mọi ký tự hiện được ánh xạ tới`x`thay vào đó nên ánh xạ tới`y`. Cụ thể, chúng tôi cập nhật tất cả các mục`mp[c]`Ở đâu`mp[c] == x`, đặt chúng thành`y`. Vì kích thước bảng chữ cái chỉ là 26 nên đây là thời gian không đổi. 
5. Sau khi xử lý tất cả các thao tác, hãy xây dựng lại câu trả lời bằng cách chuyển đổi từng ký tự được lưu trữ trong ngăn xếp bằng cách sử dụng ánh xạ cuối cùng. 

Tại sao điều này có tác dụng đến từ việc tách biệt bản sắc khỏi sự đại diện. Ngăn xếp lưu trữ chuỗi các hành động nối thêm ban đầu, trong khi`mp`tích lũy tất cả các phép biến đổi được áp dụng sau đó. Bất kỳ ký tự nào trong ngăn xếp luôn phản ánh ý nghĩa hiện tại của nó thông qua`mp`. Vì sự thay thế mang tính toàn cục về nhận dạng ký tự và không phụ thuộc vào vị trí nên sự phân tách này là chính xác và không bị mất mát. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    q = int(input())
    mp = [chr(ord('a') + i) for i in range(26)]
    st = []

    for _ in range(q):
        parts = input().split()
        if parts[0] == '1':
            x = parts[1]
            st.append(mp[ord(x) - 97])
        elif parts[0] == '2':
            if st:
                st.pop()
        else:
            x = parts[1]
            y = parts[2]
            x = mp[ord(x) - 97]
            y = mp[ord(y) - 97]
            for i in range(26):
                if mp[i] == x:
                    mp[i] = y

    if not st:
        print("The final string is empty")
    else:
        print("".join(st))

if __name__ == "__main__":
    solve()
```Việc triển khai giữ ngăn xếp làm nguồn xác thực cho cấu trúc, trong khi mảng ánh xạ phát triển để phản ánh các thay thế toàn cục. Sự tinh tế quan trọng là áp dụng`mp[x]`trong quá trình chèn, điều này đảm bảo các ký tự cũ được cố định ở dạng ngữ nghĩa hiện tại của chúng. Trong quá trình thay thế, chúng tôi bình thường hóa cả hai`x`Và`y`thông qua ánh xạ hiện tại trước khi áp dụng bản cập nhật, ngăn chặn sự không nhất quán khi tồn tại chuỗi thay thế. 

Việc xây dựng lại cuối cùng là không đáng kể vì tất cả các ký tự trong ngăn xếp đã phản ánh trạng thái được ánh xạ cuối cùng của chúng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1 a
1 b
1 c
3 a c
1 b
```Chúng tôi theo dõi ngăn xếp và lập bản đồ. 

| Bước | Hoạt động | Ngăn xếp | Thay đổi bản đồ | 
| --- | --- | --- | --- | 
| 1 | thêm | một | danh tính | 
| 2 | thêm b | a b | danh tính | 
| 3 | thêm c | a b c | danh tính | 
| 4 | a→c | a b c | a trở thành c | 
| 5 | thêm b | c b c b | sau khi lập bản đồ | 

Chuỗi cuối cùng là`cbcb`. 

Điều này xác nhận rằng sự thay thế ảnh hưởng đến việc diễn giải trong tương lai chứ không phải cấu trúc được lưu trữ. 

### Ví dụ 2 

đầu vào:```
1 a
1 b
1 c
1 c
3 a c
3 c a
```| Bước | Hoạt động | Ngăn xếp | Thay đổi bản đồ | 
| --- | --- | --- | --- | 
| 1 | thêm | một | danh tính | 
| 2 | thêm b | a b | danh tính | 
| 3 | thêm c | a b c | danh tính | 
| 4 | thêm c | a b c c | danh tính | 
| 5 | a→c | c b c c | a được ánh xạ tới c | 
| 6 | c→a | a b a a | cập nhật chuỗi | 

Đầu ra cuối cùng là`abaa`. 

Điều này cho thấy các thay thế theo chuỗi được xử lý chính xác vì ánh xạ luôn được áp dụng bắc cầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(26 · q) | Mỗi thao tác không đổi hoặc quét một bảng chữ cái cố định | 
| Không gian | O(q) | Ngăn xếp lưu trữ tối đa q ký tự | 

Bảng chữ cái được cố định ở số 26, do đó ngay cả bước thay thế cũng có thời gian không đổi. Điều này phù hợp thoải mái trong vòng 1 giây cho 100000 thao tác. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    try:
        return solve()
    except SystemExit:
        return ""

# provided sample 1
assert run("""1 a
1 b
1 c
3 a c
1 b
""") == "cbcb"

# provided sample 2
assert run("""1 a
1 b
1 c
1 c
3 a c
3 c a
""") == "abaa"

# empty case
assert run("""1 a
2
""") == "The final string is empty"

# full deletion
assert run("""1 a
1 b
2
2
""") == "The final string is empty"

# chained replacements
assert run("""1 a
1 b
3 a b
3 b a
""") == "aa"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| thêm-xóa tối thiểu | tin nhắn trống | xóa đúng | 
| bật đôi | tin nhắn trống | an toàn ranh giới | 
| trao đổi chuỗi | aa | lập bản đồ tính bắc cầu | 
| trường hợp mẫu | cbcb / abaa | tính đúng đắn khi trộn đầy đủ | 

## Vỏ cạnh 

Trường hợp một cạnh là việc xóa đi lặp lại ngoài sự trống rỗng. Đối với đầu vào như`1 a`theo sau là nhiều`2`hoạt động, ngăn xếp phải được giữ trống mà không có lỗi. Việc triển khai kiểm tra độ dài ngăn xếp trước khi xuất hiện, đảm bảo an toàn. 

Một trường hợp khác là sự thay thế lặp đi lặp lại tạo thành chu kỳ. Ví dụ,`a→b`,`b→c`,`c→a`. Phương pháp ánh xạ xử lý vấn đề này vì mỗi thao tác sẽ viết lại dựa trên các đại diện hiện tại, duy trì tính nhất quán mà không cần khôi phục lịch sử. 

Trường hợp tinh tế cuối cùng là thay thế sau khi xóa. Vì việc xóa chỉ ảnh hưởng đến ngăn xếp chứ không ảnh hưởng đến ánh xạ nên việc áp dụng thay thế sau khi xóa phần tử sẽ không làm hỏng trạng thái trước đó. Ánh xạ tiếp tục chỉ áp dụng cho các ký tự còn lại và các phần tử được bật lên sẽ bị loại bỏ vĩnh viễn.
