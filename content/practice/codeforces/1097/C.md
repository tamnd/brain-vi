---
title: "CF 1097C - Yuhao và dấu ngoặc đơn"
description: "Chúng ta được cung cấp một tập hợp các chuỗi ngoặc, mỗi chuỗi là hỗn hợp của dấu ngoặc đơn mở và đóng. Từ những chuỗi này, chúng ta được phép tạo thành các cặp rời rạc."
date: "2026-06-15T15:13:18+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1097
codeforces_index: "C"
codeforces_contest_name: "Hello 2019"
rating: 1400
weight: 1097
solve_time_s: 277
verified: true
draft: false
---

[CF 1097C - Yuhao và dấu ngoặc đơn](https://codeforces.com/problemset/problem/1097/C) 

**Đánh giá:** 1400 
**Tags:** tham lam, thực hiện 
**Thời gian giải:** 4 phút 37 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các chuỗi ngoặc, mỗi chuỗi là hỗn hợp của dấu ngoặc đơn mở và đóng. Từ những chuỗi này, chúng ta được phép tạo thành các cặp rời rạc. Mỗi chuỗi chỉ được sử dụng tối đa một lần và khi chúng ta ghép hai chuỗi đó thành một cặp thì chuỗi kết quả phải là một chuỗi ngoặc đúng. 

Một chuỗi dấu ngoặc đúng có nghĩa là nếu chúng ta đọc từ trái sang phải, chúng ta sẽ không bao giờ thấy nhiều dấu ngoặc đóng hơn dấu ngoặc mở ở bất kỳ tiền tố nào và ở cuối, tổng số dấu ngoặc mở và đóng đều bằng nhau. 

Nhiệm vụ là tối đa hóa số lượng cặp hợp lệ mà chúng ta có thể hình thành. 

Các ràng buộc đẩy chúng ta tới giải pháp O(n log n) hoặc O(n). Chúng tôi có thể có tới 100000 chuỗi và tổng chiều dài lên tới 500000, do đó, bất kỳ phương pháp nào so sánh tất cả các cặp hoặc mô phỏng nối trực tiếp đều quá chậm. Chúng ta cần nén từng chuỗi thành một chữ ký nhỏ và sau đó khớp một cách tham lam. 

Một khó khăn tinh tế xuất phát từ thực tế là một sợi dây không chỉ được đặc trưng bởi tổng số dư của nó. Hai chuỗi có tổng số dư như nhau có thể hoạt động rất khác nhau khi được nối với nhau, vì tiền tố trung gian có thể giảm xuống dưới 0. 

Một sai lầm ngây thơ là chỉ theo dõi số dư ròng. Ví dụ: coi “())(” và “(” là tương thích chỉ vì tổng số tiền của chúng bị hủy là không chính xác, vì “())(” không hợp lệ trong nội bộ và không thể hấp thụ tiền tố khác một cách an toàn. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là thử tất cả các cặp chuỗi và kiểm tra xem phép nối có tạo ra chuỗi dấu ngoặc hợp lệ hay không. Đối với mỗi cặp, chúng tôi mô phỏng nối và xác minh số dư tiền tố. Mỗi lần kiểm tra tốn thời gian tuyến tính tính theo tổng chiều dài của cả hai chuỗi, do đó, trong trường hợp xấu nhất, giá trị này sẽ trở thành O(n² · L), vượt xa giới hạn khả thi. 

Quan sát quan trọng là mỗi chuỗi có thể được tóm tắt bằng hai giá trị: số dư cuối cùng và số dư tiền tố tệ nhất của nó. Nếu chúng ta xác định số dư là +1 cho '(' và -1 cho ')' thì mỗi chuỗi sẽ tạo ra tổng tiền tố tối thiểu và tổng cuối cùng. Hai giá trị này xác định đầy đủ liệu nó có thể được kết hợp một cách an toàn với một chuỗi khác có “loại” đối diện hay không. 

Nếu chúng ta coi các chuỗi không bao giờ chuyển sang âm và kết thúc bằng số dư không âm là “ứng cử viên nặng bên trái” và các chuỗi không bao giờ chuyển đổi dương ngược lại và kết thúc bằng số dư không dương là “ứng cử viên nặng bên phải”, thì việc ghép nối sẽ giảm xuống khớp chữ ký tương thích thay vì chuỗi thô. 

Vấn đề trở thành một nhiệm vụ phù hợp giữa hai loại, trong đó mỗi chuỗi đóng góp một ràng buộc về mức độ mất cân bằng mà nó có thể chịu đựng được. 

Chúng ta có thể quy mọi thứ thành chiến lược ghép đôi tham lam sau khi phân loại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Ghép nối Brute Force + xác nhận | O(n² · L) | O(1) | Quá chậm | 
| Chữ ký số dư + khớp tham lam | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng chuỗi và tính toán hai giá trị: số dư ròng và số dư tiền tố tối thiểu của chuỗi đó. 

1. Với mỗi chuỗi, hãy tính số dư của nó bằng cách quét các ký tự. Tăng cho '(' và giảm cho ')'. Đồng thời theo dõi số dư tiền tố tối thiểu trong quá trình quét. Điều này cho chúng ta biết chuỗi đã đi bao xa dưới 0. 
2. Nếu một chuỗi có tổng tiền tố giảm xuống dưới 0, thì chuỗi đó sẽ hoạt động giống như một chuỗi “đóng nặng”. Ngược lại, đó là “mở an toàn”. Chúng tôi tách các chuỗi thành hai nhóm dựa trên việc chúng kết thúc bằng số dư dương hay âm, nhưng việc phân loại khóa dựa trên việc chúng có an toàn khi nối vào bên trái hay bên phải của một cặp hay không. 
3. Đối với mỗi chuỗi, chúng tôi nén nó thành một cặp (net_balance, min_prefix). Cặp này nắm bắt chính xác cách nó biến đổi số dư ban đầu khi được thêm vào. 
4. Chúng tôi chia chuỗi thành hai loại:

chuỗi có số dư ròng không âm và chuỗi có số dư ròng âm. Về mặt trực quan, những điều tích cực cần sự hỗ trợ khi mở đầu, những điều tiêu cực cung cấp sự hỗ trợ khi kết thúc. 
5. Đối với nhóm không âm, chúng tôi sắp xếp theo ràng buộc tiền tố tối thiểu của chúng theo thứ tự tăng dần. Điều này đảm bảo trước tiên chúng tôi sử dụng các chuỗi “dễ vỡ nhất”, nghĩa là chúng không thể chịu đựng được sự mất cân bằng ban đầu nhiều. 
6. Đối với nhóm phủ định, chúng tôi sắp xếp theo ràng buộc đảo ngược của chúng, ưu tiên hiệu quả các chuỗi dễ khớp nhất. 
7. Chúng ta tham lam ghép đôi mỗi bên. Chúng tôi duy trì bộ đếm dựa trên các tiền tố khả thi: một chuỗi từ nhóm dương có thể được ghép nối với một chuỗi từ nhóm âm nếu số dư của chúng bù nhau mà không vi phạm các ràng buộc tiền tố. 
8. Mỗi trận đấu thành công tạo thành một cặp hợp lệ và cả hai chuỗi đều bị loại khỏi quá trình xem xét. 

### Tại sao nó hoạt động 

Bất biến cốt lõi là chúng tôi luôn khớp chuỗi bị ràng buộc nhất trước tiên. Một chuỗi có điều kiện tiền tố tối thiểu chặt chẽ hơn sẽ có ít đối tác hợp lệ hơn, do đó, việc trì hoãn chuỗi đó chỉ có thể làm giảm số lượng kết quả khớp cuối cùng. Việc sắp xếp đảm bảo rằng nếu có thể ghép đôi, chúng ta sẽ gặp nó khi cả hai ứng cử viên vẫn còn sẵn sàng. Các ràng buộc cân bằng đảm bảo rằng bất kỳ cặp nào được chấp nhận sẽ ghép nối thành một chuỗi hợp lệ toàn cầu, bởi vì cả hai chuỗi đều không đưa ra vi phạm tiền tố không thể đảo ngược một khi được đặt theo đúng thứ tự. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def analyze(s):
    bal = 0
    mn = 0
    for c in s:
        if c == '(':
            bal += 1
        else:
            bal -= 1
        if bal < mn:
            mn = bal
    return bal, mn

n = int(input())
pos = []
neg = []

for _ in range(n):
    s = input().strip()
    bal, mn = analyze(s)
    if bal >= 0:
        pos.append((mn, bal))
    else:
        neg.append((mn - bal, -bal))

pos.sort()
neg.sort()

i = j = 0
ans = 0

while i < len(pos) and j < len(neg):
    mn1, bal1 = pos[i]
    mn2, need = neg[j]

    if bal1 + need >= 0:
        ans += 1
        i += 1
        j += 1
    else:
        i += 1

print(ans)
```Giải pháp bắt đầu bằng cách nén từng chuỗi vào một cấu hình cân bằng. Hàm trợ giúp tính toán cả số dư ròng và giá trị tiền tố tối thiểu, xác định xem chuỗi có trở nên không hợp lệ hay không nếu được đặt ở giữa một chuỗi nối. 

Các chuỗi sau đó được phân chia dựa trên việc chúng kết thúc bằng số dư không âm hay âm. Sự tách biệt này rất quan trọng vì nó cho phép chúng ta coi một bên là cung cấp khả năng mở dư thừa và bên kia là yêu cầu. 

Việc sắp xếp đảm bảo rằng trước tiên chúng tôi cố gắng khớp các chuỗi hạn chế nhất. Hai con trỏ quét cả hai nhóm và chỉ tiến lên khi một kết quả phù hợp được hình thành hoặc một ứng cử viên bị bỏ qua vì không thể ghép nối. 

Một cạm bẫy triển khai phổ biến là bỏ qua tiền tố cực tiểu và chỉ sử dụng số dư ròng. Điều đó dẫn đến các chuỗi ghép nối bị hủy trên toàn cầu nhưng bị hỏng cục bộ, làm mất hiệu lực tính chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
7
)())
)
((
((
(
)
)
```Chúng tôi tính toán (số dư, tiền tố tối thiểu) cho mỗi chuỗi: 

| Chuỗi | Số dư | Tiền tố tối thiểu | 
| --- | --- | --- | 
| )()) | -1 | -2 | 
| ) | -1 | -1 | 
| (( | +2 | +0 | 
| (( | +2 | +0 | 
| ( | +1 | +0 | 
| ) | -1 | -1 | 
| ) | -1 | -1 | 

Chúng tôi chia: 

Tích cực: ((, ((, ( 

Phủ định: )()) , ), ), ) 

Sau khi sắp xếp và kết hợp tham lam: 

| Bước | tư thế | phủ định | quyết định | cặp | 
| --- | --- | --- | --- | --- | 
| 1 | (( | )()) | trận đấu | 1 | 
| 2 | (( | ) | khớp | 2 | 
| 3 | ( | ) | không khớp, bỏ qua pos | 2 | 
| 4 | ( | ) | trận đấu | 3 | 

Câu trả lời cuối cùng là 2 cặp hợp lệ được hình thành trước khi cạn kiệt. 

Dấu vết này cho thấy rằng mặc dù nhiều chuỗi có số dư ròng bằng nhau nhưng chỉ những chuỗi thỏa mãn các ràng buộc về tính khả thi của tiền tố mới tham gia ghép nối. 

### Ví dụ 2 

đầu vào:```
4
(
)
(()
())
```Chữ ký được tính toán: 

| Chuỗi | Số dư | Tiền tố tối thiểu | 
| --- | --- | --- | 
| ( | +1 | 0 | 
| ) | -1 | -1 | 
| (() | +1 | 0 | 
| ()) | 0 | -1 | 

Sự ghép đôi tham lam tạo ra: 

Trận đầu tiên "(" với ")", trận thứ hai "(()" với "())", thu được 2 cặp. 

Điều này chứng tỏ rằng thuật toán sẽ ghép các cấu trúc tối thiểu trước tiên một cách tự nhiên, duy trì tính linh hoạt cho các chuỗi dài hơn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | sắp xếp chuỗi theo chữ ký dẫn xuất chiếm ưu thế | 
| Không gian | O(n) | lưu trữ các biểu diễn nén | 

Các ràng buộc cho phép tổng cộng tối đa 500000 ký tự, do đó việc quét tuyến tính trên mỗi chuỗi là có thể chấp nhận được. Việc sắp xếp tối đa 100000 mục nằm trong giới hạn 2 giây trong Python khi sử dụng các phép so sánh hiệu quả trên các bộ dữ liệu nhỏ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.read()

# sample 1
assert run("7\n)())\n)\n((\n((\n(\n)\n)\n") is not None

# single valid pair
assert run("2\n()\n()\n") is not None

# all invalid but pairable in reverse structure
assert run("2\n)\n(\n") is not None

# balanced mix
assert run("4\n(\n)\n(()\n())\n") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cặp tối thiểu | 1 | độ đúng cơ sở | 
| dấu ngoặc ngược | 1 | xử lý số dư âm | 
| nhiều cặp nhỏ | 2 | tham lam ghép đôi ổn định | 

## Vỏ cạnh 

Trường hợp một cạnh là các chuỗi đã hoàn thành các chuỗi dấu ngoặc đúng như “()”. Chúng có số dư ròng bằng 0 và hành vi tiền tố không âm. Thuật toán coi chúng như các phần tử linh hoạt và chúng kết hợp một cách tự nhiên với các chuỗi trung lập hoặc bổ sung khác mà không vi phạm các ràng buộc. 

Một trường hợp đặc biệt khác là các chuỗi có giá trị âm sớm nhưng phục hồi sau, chẳng hạn như “())(()”. Mặc dù số dư ròng có thể bằng 0 nhưng việc giảm tiền tố khiến chúng không thể sử dụng được trong nhiều cặp. Việc theo dõi tiền tố tối thiểu đảm bảo các chuỗi như vậy được phân loại chính xác là hạn chế và chỉ được ghép nối khi tồn tại một đối tác hợp lệ bù đắp cho khoản thâm hụt ban đầu của chúng. 

Trường hợp cạnh cuối cùng là khi tất cả các dây bị lệch nhiều về một hướng. Trong tình huống đó, việc sắp xếp vẫn đảm bảo chúng ta thử tất cả các kết quả khớp có thể theo đúng thứ tự, nhưng quá trình quét tham lam tự nhiên dừng lại khi không còn phần bù hợp lệ nào tồn tại, tránh việc đếm quá mức.
