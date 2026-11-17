---
title: "CF 2A - Người chiến thắng"
description: "Trò chơi ghi lại một chuỗi các vòng chơi. Trong mỗi vòng, một người chơi được hoặc mất một số điểm. Cuối cùng, người chơi có tổng điểm cao nhất sẽ giành chiến thắng."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "hashing", "implementation"]
categories: ["algorithms"]
codeforces_contest: 2
codeforces_index: "A"
codeforces_contest_name: "Codeforces Beta Round 2"
rating: 1500
weight: 2
solve_time_s: 69
verified: true
draft: false
---
## Hiểu vấn đề 

Trò chơi ghi lại một chuỗi các vòng chơi. Trong mỗi vòng, một người chơi được hoặc mất một số điểm. Cuối cùng, người chơi có tổng điểm cao nhất sẽ giành chiến thắng. 

Phần khó khăn xuất hiện khi có nhiều người chơi về đích với cùng số điểm cao nhất. Trong tình huống đó, người chiến thắng không được chọn một cách tùy tiện. Trong số tất cả những người chơi có điểm cuối cùng bằng giá trị tối đa, chúng tôi chọn người đạt được điểm đó đầu tiên trong trò chơi. 

Đầu vào đã được cung cấp theo thứ tự thời gian, vì vậy thứ tự của các vòng rất quan trọng. Mỗi dòng chứa tên người chơi và sự thay đổi điểm số của vòng đó. Điểm có thể trở thành âm trong suốt trò chơi và người chơi có thể di chuyển lên trên hoặc xuống dưới điểm chiến thắng cuối cùng nhiều lần. 

Các ràng buộc đủ nhỏ để hầu hết mọi cách triển khai hợp lý đều có thể vượt qua. Có nhiều nhất là 1000 vòng, nên ngay cả một$O(n^2)$giải pháp chỉ thực hiện khoảng một triệu hoạt động. Điều đó dễ dàng phù hợp trong thời hạn. Tuy nhiên, vấn đề không nằm ở việc tối ưu hóa mà nằm ở việc xử lý chính xác quy tắc ràng buộc. 

Sai lầm phổ biến nhất là chỉ quyết định người chiến thắng dựa trên điểm số cuối cùng. Hãy xem xét đầu vào này:```
4
alice 5
bob 5
alice -2
bob -2
```Cả hai tay vợt đều kết thúc với 3 điểm. Alice đạt được 3 đầu tiên, vì sau nước đi đầu tiên cô đã có 5. Bob đạt 3 sau đó. Câu trả lời đúng là:```
alice
```Giải pháp bất cẩn mà chỉ kiểm tra tổng số cuối cùng thì không thể phân biệt được trường hợp này. 

Một trường hợp tinh tế khác xảy ra khi người chơi đạt được điểm mục tiêu sớm nhưng không về đích với số điểm tối đa.```
5
alice 10
bob 3
alice -8
bob 5
bob 2
```Điểm cuối cùng là:```
alice = 2
bob = 10
```Alice đạt điểm 10 đầu tiên, nhưng cô ấy không đủ điều kiện vì điểm cuối cùng của cô ấy không phải là điểm tối đa. Người chiến thắng là:```
bob
```Việc triển khai sai có thể chọn không chính xác người chơi đầu tiên chạm vào số điểm tối đa. 

Ngoài ra còn có một tương tác quan trọng với các giá trị âm:```
5
alice 2
bob 3
alice 3
bob -1
alice -2
```Điểm số cuối cùng:```
alice = 3
bob = 2
```Điểm cuối cùng tối đa là 3. Alice trở thành người chiến thắng mặc dù trước đó cô ấy tạm thời có 5 điểm. Quy tắc dựa trên việc đạt được ít nhất số điểm tối đa cuối cùng, không nhất thiết phải khớp chính xác với số điểm đó ở cuối một số vòng chơi. 

## Phương pháp tiếp cận 

Giải pháp brute-force có thể mô phỏng toàn bộ trò chơi và liên tục tính lại điểm của mọi người chơi sau mỗi vòng. Đối với mỗi vòng, chúng tôi có thể quét tất cả các vòng trước để xác định tổng số hiện tại, sau đó cố gắng quyết định xem ai đó đã đủ điều kiện trở thành người chiến thắng hay chưa. 

Điều này hoạt động vì các ràng buộc rất nhỏ. Với$n = 1000$, MỘT$O(n^2)$giải pháp thực hiện khoảng một triệu bản cập nhật, điều này hoàn toàn có thể chấp nhận được. 

Vấn đề với cách tiếp cận này không phải là hiệu suất mà là sự rõ ràng. Điều kiện hòa phụ thuộc vào hai khái niệm khác nhau về điểm số: 

Đầu tiên, điểm tối đa cuối cùng của tất cả người chơi. 

Thứ hai, thời điểm sớm nhất mà người chơi cuối cùng đạt được mức tối đa đó trong trò chơi. 

Cố gắng tính toán cả hai cùng một lúc sẽ trở nên lộn xộn. 

Quan sát rõ ràng hơn là vấn đề được chia thành hai phần một cách tự nhiên. 

Trong lần vượt qua đầu tiên, chúng tôi tính toán điểm cuối cùng của mọi người chơi. Khi đường chuyền này kết thúc, chúng tôi biết điểm cuối cùng tối đa$mx$. 

Giờ đây luật hòa đã trở nên đơn giản hơn rất nhiều. Chúng tôi chỉ quan tâm đến những người chơi có điểm cuối cùng bằng nhau$mx$. Trong số những người chơi đó, chúng tôi muốn người đầu tiên có điểm chạy ít nhất$mx$. 

Điều đó dẫn đến đường chuyền thứ hai rất trực tiếp. Chúng tôi phát lại trò chơi theo trình tự thời gian trong khi vẫn duy trì tổng số lượt chạy. Khi người chơi thỏa mãn cả hai điều kiện: 

1. Điểm cuối cùng của họ bằng nhau$mx$. 
2. Điểm chạy của họ ít nhất là$mx$. 

người chơi đó phải là câu trả lời. 

Điều kiện thứ hai sử dụng "ít nhất" thay vì "bằng" vì người chơi có thể vượt quá điểm mục tiêu trước khi kết thúc chính xác ở điểm đó sau đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Đã chấp nhận | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các vòng và lưu chúng vào danh sách vì chúng ta cần xử lý trò chơi hai lần. 
2. Trong lần vượt qua đầu tiên, hãy duy trì một từ điển`final_score[name]`lưu trữ tổng số điểm của mỗi người chơi sau tất cả các vòng đấu. 
3. Sau khi xử lý mỗi vòng, hãy tính điểm cuối cùng tối đa`mx`. 
4. Bắt đầu lượt thứ hai qua các vòng theo thứ tự thời gian. Duy trì một từ điển khác`current_score[name]`đại diện cho điểm số trong quá trình chơi trò chơi. 
5. Đối với mỗi vòng, hãy cập nhật điểm chạy của người chơi đó. 
6. Sau khi cập nhật điểm chạy, hãy kiểm tra hai điều kiện: 

1. Điểm cuối cùng của người chơi bằng`mx`. 
2. Điểm chạy của người chơi ít nhất là`mx`. 
7. Người chơi đầu tiên đáp ứng được cả hai điều kiện là người chiến thắng. In tên của họ và dừng lại ngay lập tức. 

Lý do điều này có tác dụng là vì chỉ những người chơi có điểm cuối cùng`mx`có đủ điều kiện để giành chiến thắng. Vòng thứ hai phát hiện xem ai trong số họ đến được mục tiêu sớm nhất vì các vòng được xử lý theo trình tự thời gian. 

### Tại sao nó hoạt động 

Sau lần vượt qua đầu tiên,`mx`là số điểm cao nhất đạt được khi kết thúc trò chơi. Bất kỳ người chơi nào có điểm cuối cùng nhỏ hơn đều không thể giành chiến thắng, bất kể điểm tạm thời của họ trước đó lớn đến mức nào. 

Trong lần vượt qua thứ hai,`current_score[name]`luôn bằng điểm của người chơi đó sau vòng đấu hiện tại. Người chơi đủ điều kiện đầu tiên có tổng số lần chạy ít nhất`mx`chính xác là người chơi đạt ngưỡng chiến thắng đầu tiên trong trò chơi. 

Vì các vòng được xử lý theo thứ tự nên không người chơi nào sau này có thể đạt đến ngưỡng sớm hơn. Điều đó đảm bảo tính đúng đắn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())

rounds = []
final_score = {}

for _ in range(n):
    name, score = input().split()
    score = int(score)

    rounds.append((name, score))

    if name not in final_score:
        final_score[name] = 0

    final_score[name] += score

mx = max(final_score.values())

current_score = {}

for name, score in rounds:
    if name not in current_score:
        current_score[name] = 0

    current_score[name] += score

    if final_score[name] == mx and current_score[name] >= mx:
        print(name)
        break
```Giải pháp tuân theo cấu trúc hai lượt trực tiếp. 

Vòng lặp đầu tiên lưu trữ mỗi vòng và tính tổng số cuối cùng. Việc giữ các vòng trong danh sách là cần thiết vì lượt thứ hai phải chơi lại trận đấu theo trình tự thời gian. 

Từ điển`final_score`ánh xạ từng người chơi tới điểm số của họ sau tất cả các vòng đấu. Khi lượt đầu tiên kết thúc,`mx`trở thành điểm số chiến thắng cuối cùng. 

Vòng lặp thứ hai xây dựng lại trò chơi từng bước. Từ điển`current_score`theo dõi điểm số trong quá trình chơi trò chơi thay vì tổng số cuối cùng. 

Thứ tự của các hoạt động quan trọng. Trước tiên, chúng tôi cập nhật điểm chạy, sau đó kiểm tra xem người chơi có đủ điều kiện hay không. Việc kiểm tra trước khi cập nhật sẽ làm dịch chuyển từng điểm một vòng và tạo ra câu trả lời sai. 

Điều kiện:```
current_score[name] >= mx
```là tinh tế nhưng cần thiết. Giả sử điểm thắng là 5 và người chơi tạm thời đạt 7 trước khi kết thúc ở 5 sau đó. Họ vẫn được tính là đã đạt được mục tiêu trước đó. 

Việc ngắt ngay sau khi tìm được người chơi hợp lệ đầu tiên là đúng vì các vòng đấu được xử lý theo trình tự thời gian. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
mike 3
andrew 5
mike 2
```Vượt qua đầu tiên: 

| Vòng | Người chơi | Đồng bằng | Điểm cuối cùng | 
| --- | --- | --- | --- | 
| 1 | mike | +3 | mik = 3 | 
| 2 | Andrew | +5 | Mike = 3, Andrew = 5 | 
| 3 | mike | +2 | mike = 5, andrew = 5 | 

Điểm cuối cùng tối đa là 5. 

Vượt qua thứ hai: 

| Vòng | Người chơi | Điểm chạy | Đạt chuẩn? | 
| --- | --- | --- | --- | 
| 1 | mike | 3 | Không | 
| 2 | Andrew | 5 | Có | 

Đầu ra:```
andrew
```Dấu vết này cho thấy lý do tại sao đường chuyền thứ hai hoạt động. Cả hai người chơi đều kết thúc với 5, nhưng Andrew đạt 5 sớm hơn trong trò chơi. 

### Ví dụ 2 

đầu vào:```
5
alice 10
bob 3
alice -8
bob 5
bob 2
```Vượt qua đầu tiên: 

| Vòng | Người chơi | Đồng bằng | Điểm cuối cùng | 
| --- | --- | --- | --- | 
| 1 | Alice | +10 | Alice = 10 | 
| 2 | bob | +3 | Alice = 10, Bob = 3 | 
| 3 | Alice | -8 | alice = 2, bob = 3 | 
| 4 | bob | +5 | Alice = 2, Bob = 8 | 
| 5 | bob | +2 | Alice = 2, Bob = 10 | 

Điểm cuối cùng tối đa là 10. 

Vượt qua thứ hai: 

| Vòng | Người chơi | Điểm chạy | Đạt chuẩn? | 
| --- | --- | --- | --- | 
| 1 | Alice | 10 | Không, điểm cuối cùng chỉ là 2 | 
| 2 | bob | 3 | Không | 
| 3 | Alice | 2 | Không | 
| 4 | bob | 8 | Không | 
| 5 | bob | 10 | Có | 

Đầu ra:```
bob
```Ví dụ này cho thấy trường hợp cạnh quan trọng nhất. Đạt được số điểm tối đa trong trò chơi là chưa đủ. Người chơi cũng phải về đích với số điểm tối đa đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi vòng được xử lý hai lần | 
| Không gian | O(n) | Từ điển và vòng lưu trữ sử dụng không gian tuyến tính | 

Với tối đa 1000 vòng, giải pháp nằm trong giới hạn thoải mái. Ngay cả những cách tiếp cận chậm hơn nhiều cũng sẽ vượt qua, nhưng giải pháp tuyến tính sẽ rõ ràng hơn và phù hợp với cấu trúc của vấn đề một cách tự nhiên. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    n = int(input())

    rounds = []
    final_score = {}

    for _ in range(n):
        name, score = input().split()
        score = int(score)

        rounds.append((name, score))

        final_score[name] = final_score.get(name, 0) + score

    mx = max(final_score.values())

    current_score = {}

    for name, score in rounds:
        current_score[name] = current_score.get(name, 0) + score

        if final_score[name] == mx and current_score[name] >= mx:
            print(name)
            return

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    output = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return output.strip()

# provided sample
assert run(
"""3
mike 3
andrew 5
mike 2
"""
) == "andrew", "sample 1"

# minimum input size
assert run(
"""1
alice 10
"""
) == "alice", "single round"

# tie resolved by earliest reach
assert run(
"""4
alice 5
bob 5
alice -2
bob -2
"""
) == "alice", "earliest reach wins"

# temporary high score should not matter
assert run(
"""5
alice 10
bob 3
alice -8
bob 5
bob 2
"""
) == "bob", "must finish with maximum"

# negative values and multiple crossings
assert run(
"""6
a 2
b 3
a 3
b 2
a -2
b -1
"""
) == "b", "running totals handled correctly"

# all players equal final score
assert run(
"""6
x 1
y 2
x 1
y 0
x 0
y 0
"""
) == "y", "first to reach maximum among tied players"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Trò chơi vòng đơn | Alice | Kích thước đầu vào tối thiểu | 
| Hòa với trận chung kết ngang nhau | Alice | Quy tắc tiếp cận sớm nhất | 
| Điểm cao tạm thời | bob | Tính đủ điều kiện về điểm cuối cùng | 
| Chuyển đổi điểm âm | b | Xử lý đúng các khoản giảm | 
| Tổng số cuối cùng bằng nhau | y | Bứt phá theo trình tự thời gian | 

## Vỏ cạnh 

Hãy xem xét trường hợp hai người chơi về đích với cùng số điểm nhưng một người về đích sớm hơn:```
4
alice 5
bob 5
alice -2
bob -2
```Điểm cuối cùng là:```
alice = 3
bob = 3
```Điểm cuối cùng tối đa là 3. 

Trong lần vượt qua thứ hai:```
alice reaches 5 after round 1
bob reaches 5 after round 2
```Điểm chạy của cả hai đều đạt ít nhất là 3 khi chúng xuất hiện lần đầu, nhưng Alice xuất hiện sớm hơn theo trình tự thời gian. Thuật toán in ngay lập tức`alice`. 

Bây giờ hãy xem xét một người chơi tạm thời dẫn trước nhưng không về đích trước:```
5
alice 10
bob 3
alice -8
bob 5
bob 2
```Điểm cuối cùng tối đa là 10. Trong khi chơi lại, Alice đạt 10 đầu tiên, nhưng điểm cuối cùng của cô ấy chỉ là 2. Điều kiện:```
final_score[name] == mx
```từ chối cô ấy một cách chính xác. Cuối cùng Bob đạt đến điểm 10 và giành chiến thắng. 

Cuối cùng, hãy xem xét việc lặp đi lặp lại điểm mục tiêu:```
6
a 2
b 3
a 3
b 2
a -2
b -1
```Điểm số cuối cùng:```
a = 3
b = 4
```Tỷ số cuối cùng giành chiến thắng là 4. 

Phát lại:```
a: 2
b: 3
a: 5
b: 5
```Khi`b`đạt 5, điểm chạy ít nhất là 4 và điểm cuối cùng bằng 4, vì vậy`b`thắng ngay lập tức. Thuật toán không quan tâm đến điều đó`a`vượt quá 4 trước đó vì`a`đã không kết thúc với tổng số chiến thắng.
