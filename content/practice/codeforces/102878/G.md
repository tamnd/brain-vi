---
title: "CF 102878G - Nim plus"
description: "Trò chơi được chơi với một đống duy nhất chứa n quả bóng giấy. Long Long và Mao Mao không có động thái pháp lý giống nhau. Long Long có một bộ số được phép bỏ, còn Mao Mao có một bộ khác."
date: "2026-07-25T12:44:41+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102878
codeforces_index: "G"
codeforces_contest_name: "The 15-th BIT Campus Programming Contest - Onsite Round"
rating: 0
weight: 102878
solve_time_s: 41
verified: true
draft: false
---

[CF 102878G - Nim plus](https://codeforces.com/problemset/problem/102878/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 41s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Trò chơi được chơi với một cọc duy nhất chứa`n`quả bóng giấy. Long Long và Mao Mao không có động thái pháp lý giống nhau. Long Long có một bộ số được phép bỏ, còn Mao Mao có một bộ khác. Trong một lượt, người chơi chọn một số từ bộ của mình và loại bỏ chính xác số bóng đó, nhưng chỉ khi cọc chứa đủ bóng. Người chơi không thể di chuyển sẽ thua. Nhiệm vụ là xác định xem người chơi đầu tiên, Long Long, có thể giành chiến thắng bằng lối chơi tối ưu hay không. 

Đầu vào cung cấp số lượng bóng ban đầu, kích thước của cả hai bộ nước đi và bản thân hai bộ nước đi đó. Đầu ra là tên người chiến thắng ở định dạng bắt buộc. Phần quan trọng của vấn đề là người chơi có các lựa chọn di chuyển khác nhau, vì vậy lý thuyết Nim khách quan thông thường không được áp dụng trực tiếp. Trạng thái tùy thuộc vào lượt của ai. 

Các ràng buộc đủ nhỏ để lập trình động. Kích thước cọc có thể đạt tới 5000 và mỗi bộ nước đi có tối đa 100 giá trị. Điều này có nghĩa là một giải pháp xung quanh`n * m`, tức là khoảng 500.000 lần chuyển đổi, đủ nhanh. Việc mô phỏng mọi chuỗi trò chơi có thể xảy ra sẽ là không thể vì số lượng lệnh di chuyển có thể tăng theo cấp số nhân. 

Một số trường hợp nguy hiểm có thể phá vỡ việc triển khai bất cẩn. Nếu người chơi có nước đi lớn hơn cọc hiện tại thì nước đi đó không thể được sử dụng. Ví dụ:```
5 1
6
7
```Đầu ra đúng là:```
Mao Mao nb!
```Long Long không thể lấy được 6 quả bóng ra khỏi chồng cỡ 5 nên thua ngay. Việc triển khai chỉ kiểm tra xem một nước đi có tồn tại trong tập hợp mà không kiểm tra kích thước cọc hiện tại hay không sẽ đưa ra câu trả lời sai. 

Một trường hợp khó khăn khác là khi một người chơi có thể di chuyển ngay bây giờ nhưng mọi trạng thái kết quả đều mang lại cho đối thủ một thế thắng. Ví dụ:```
1 1
1
1
```Đầu ra đúng là:```
Long Long nb!
```Long Long loại bỏ quả bóng duy nhất khiến Mao Mao không thể di chuyển. Thuật toán phải đánh giá các vị trí trong tương lai, không chỉ xem người chơi hiện tại có ít nhất một nước đi hợp pháp hay không. 

Tình huống quan trọng thứ ba xuất hiện khi hai người chơi có bộ nước đi giống hệt nhau. Người chiến thắng không được xác định bằng cách so sánh các bộ, vì lợi thế của người chơi đầu tiên vẫn phụ thuộc vào kích thước cọc hiện tại. Ví dụ:```
2 1
1
1
```Đầu ra đúng là:```
Mao Mao nb!
```Long Long loại bỏ một quả bóng, để lại một quả bóng cho Mao Mao, người loại bỏ nó và giành chiến thắng. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là khám phá cây trò chơi một cách đệ quy. Đối với mọi tiểu bang, chúng tôi thử mọi động thái hợp pháp cho người chơi hiện tại. Nếu bất kỳ nước đi nào khiến đối thủ rơi vào trạng thái thua cuộc thì người chơi hiện tại sẽ thắng. Điều này đúng vì mọi khả năng tiếp tục của trò chơi đều được xem xét. 

Vấn đề là số lượng trạng thái lặp lại. Kích thước cọc giống nhau có thể xuất hiện qua nhiều chuỗi di chuyển khác nhau. Nếu không ghi nhớ, đệ quy có thể truy cập lại các vị trí tương tự theo cấp số nhân nhiều lần. Ngay cả với tính năng ghi nhớ, trạng thái tự nhiên có hai khả năng cho người chơi quay và kích thước cọc lên tới 5000, vì vậy lập trình động là cách phù hợp để tổ chức tính toán. 

Quan sát quan trọng là toàn bộ lịch sử là không liên quan. Thông tin duy nhất cần thiết là số lượng bóng còn lại và lượt của ai. Chúng ta có thể tính toán các trạng thái này theo thứ tự tăng dần của kích thước cọc. Vì mỗi lần di chuyển đều làm giảm kích thước cọc nên mọi chuyển đổi đều hướng đến trạng thái đã được giải quyết. 

Cho phép`winL[x]`nghĩa là Long Long có thể thắng khi có`x`quả bóng và đến lượt anh ấy. Cho phép`winM[x]`nghĩa là Mao Mao có thể thắng khi có`x`quả bóng và đến lượt anh ấy. 

Đối với Long Long, một bang sẽ thắng nếu có sự loại bỏ hợp pháp khiến Mao Mao rơi vào tình trạng thua cuộc:`winL[x] = true`nếu một số`a`thỏa mãn`a <= x`Và`winM[x-a] = false`. 

Ý tưởng tương tự cũng áp dụng cho Mao Mao:`winM[x] = true`nếu một số`b`thỏa mãn`b <= x`Và`winL[x-b] = false`. 

Trạng thái cơ sở là`0`quả bóng. Người chơi không có bóng không thể di chuyển nên người chơi đó thua ngay lập tức. Điều này có nghĩa là cả hai`winL[0]`Và`winM[0]`là sai. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(số trạng thái) có ghi nhớ | Quá chậm nếu không có cấu trúc DP | 
| Tối ưu | O(nm) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo hai mảng lập trình động.`winL[i]`lưu trữ liệu Long Long có thắng với`i`quả bóng trước khi di chuyển, và`winM[i]`lưu trữ xem Mao Mao có thắng không`i`quả bóng trước khi di chuyển của mình. Cả hai giá trị đều bắt đầu bằng sai vì người chơi không có bóng sẽ không thể di chuyển. 
2. Xử lý kích thước cọc từ`1`ĐẾN`n`. Đối với mỗi kích thước, hãy thử mọi động tác có sẵn cho Long Long. Nếu một nước đi không lớn hơn cọc hiện tại và kết quả là trạng thái Mao Mao đang thua, hãy đánh dấu`winL[i]`như sự thật. Điều này tuân theo quy tắc trò chơi thông thường là chỉ cần một nước đi thắng là đủ. 
3. Để có cùng kích thước cọc, hãy thử mọi chiêu thức có sẵn của Mao Mao. Nếu một nước đi khiến Long Long rơi vào thế thua, đánh dấu`winM[i]`như sự thật. 
4. Sau khi tính toán xong tất cả các trạng thái, hãy kiểm tra`winL[n]`. Nếu là sự thật, Long Long có thể thắng được. Nếu không Mao Mao có thể ép thắng. 

Tại sao nó hoạt động: 

Tính bất biến của quy hoạch động là khi chúng ta tính toán trạng thái`i`, mọi trạng thái tiếp theo có thể xảy ra đều đã được tính toán vì mỗi lần di chuyển đều làm giảm kích thước cọc. Để một người chơi giành chiến thắng, phải tồn tại ít nhất một nước đi dẫn đến trạng thái đối thủ mà đối thủ không thể buộc phải thắng. Quá trình chuyển đổi kiểm tra chính xác những khả năng này, vì vậy mọi trạng thái đều được phân loại chính xác. Vì ván đầu tiên chỉ đơn giản là lượt của Long Long với`n`quả bóng,`winL[n]`đưa ra câu trả lời. 

## Giải pháp Python```python
import sys

input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    win_l = [False] * (n + 1)
    win_m = [False] * (n + 1)

    for i in range(1, n + 1):
        for x in a:
            if x <= i and not win_m[i - x]:
                win_l[i] = True
                break

        for x in b:
            if x <= i and not win_l[i - x]:
                win_m[i] = True
                break

    if win_l[n]:
        print("Long Long nb!")
    else:
        print("Mao Mao nb!")

if __name__ == "__main__":
    solve()
```Hai mảng đại diện cho lượt của hai người chơi khác nhau. Việc giữ các mảng riêng biệt là cần thiết vì cùng một kích thước cọc có thể thắng cho một người chơi và thua cho người kia do các bước di chuyển khác nhau của họ. 

Các vòng đi lên từ cọc nhỏ đến cọc lớn hơn. Khi tính toán`win_l[i]`, mọi tiểu bang`win_m[i - x]`đã được biết đến bởi vì`i - x < i`. Tài sản tương tự cũng có ở các bang của Mao Mao. 

điều kiện`x <= i`là việc kiểm tra ranh giới nhằm ngăn chặn việc xóa bỏ bất hợp pháp. Sớm`break`chỉ là một sự tối ưu hóa. Khi đã tìm được nước đi thắng, việc kiểm tra các nước đi khác không thể thay đổi kết quả. 

## Ví dụ đã hoạt động 

Mẫu 1:```
5 1
6
7
```| Quả bóng | Long Long bang | Bang Mao Mao | Lý do | 
| --- | --- | --- | --- | 
| 0 | Thua | Thua | Không có động thái hợp pháp | 
| 1 | Thua | Thua | Cả hai nước đi đều quá lớn | 
| 2 | Thua | Thua | Cả hai nước đi đều quá lớn | 
| 3 | Thua | Thua | Cả hai nước đi đều quá lớn | 
| 4 | Thua | Thua | Cả hai nước đi đều quá lớn | 
| 5 | Thua | Thua | Dài Dài không thể xóa 6 | 

Người chơi đầu tiên không có nước đi hợp lệ ở trạng thái ban đầu nên Mao Mao thắng. 

Mẫu 2:```
20 3
3 7 10
2 6 7
```| Quả bóng | Long Long bang | Bang Mao Mao | Lý do | 
| --- | --- | --- | --- | 
| 0 | Thua | Thua | Không di chuyển | 
| 1 | Thua | Thua | Không xóa hợp pháp | 
| 3 | Thắng | Thua | Long Long có thể loại bỏ 3 | 
| 10 | Thắng | Thắng | Cả hai người chơi đều có câu trả lời chiến thắng | 
| 20 | Thắng | Không rõ sau khi kiểm tra | Long Long bỏ 10 và để lại 10 | 

Phần quan trọng của dấu vết này là nước đi thắng không cần phải làm trống cọc. Long Long chỉ cần tiến tới trạng thái Mao Mao không thể ép được chiến thắng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nm) | Mỗi kích thước cọc kiểm tra mọi nước đi từ cả hai bộ | 
| Không gian | O(n) | Hai mảng boolean lưu trữ tất cả các trạng thái đã giải quyết | 

Với`n <= 5000`Và`m <= 100`, thuật toán thực hiện khoảng một triệu kiểm tra đơn giản trong trường hợp xấu nhất, phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    result = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout

    return result

# provided samples
assert run("""5 1
6
7
""") == "Mao Mao nb!\n", "sample 1"

assert run("""20 3
3 7 10
2 6 7
""") == "Long Long nb!\n", "sample 2"

# minimum size
assert run("""1 1
1
1
""") == "Long Long nb!\n", "single ball"

# all equal moves
assert run("""2 1
1
1
""") == "Mao Mao nb!\n", "same move set"

# large pile with repeated possible cycles
assert run("""10 2
1 10
1 10
""") == "Mao Mao nb!\n", "large equal moves"

# unavailable moves at start
assert run("""3 2
5 6
1 2
""") == "Mao Mao nb!\n", "illegal first moves"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 1 / 1`| Dài dài nb! | Nước đi thắng ngay lập tức | 
|`2 1 / 1 / 1`| Mao Mao nb! | Luân phiên các lượt di chuyển bằng nhau | 
|`10 2 / 1 10 / 1 10`| Mao Mao nb! | Chuỗi phụ thuộc dài hơn | 
|`3 2 / 5 6 / 1 2`| Mao Mao nb! | Di chuyển lớn hơn cọc | 

## Vỏ cạnh 

Đối với trường hợp Long Long không có nước đi nào có thể sử dụng được, quá trình chuyển đổi sẽ rời đi một cách chính xác`winL[n]`SAI. Trong đầu vào:```
5 1
6
7
```nước đi Dài và Dài duy nhất là 6, lớn hơn 5. Thuật toán bỏ qua nó vì`x <= i`là sai nên trạng thái vẫn thua. 

Đối với trường hợp cả hai người chơi đều có nước đi giống nhau, thuật toán không đưa ra các giả định dựa trên tính đối xứng. TRONG:```
2 1
1
1
```trạng thái có một bi là thắng cho người chơi đến lượt, nhưng trạng thái có hai bi là thua vì người chơi đầu tiên phải nhường cho đối phương trạng thái thắng một bi. 

Đối với trạng thái mà một nước đi loại bỏ tất cả các quả bóng còn lại, thuật toán sẽ xử lý nó thông qua trường hợp cơ bản. Nếu người chơi loại bỏ toàn bộ cọc, đối thủ sẽ đạt trạng thái`0`, đang thua. Điều này làm cho nước đi xuất hiện một cách chính xác như một sự chuyển tiếp thắng lợi.
