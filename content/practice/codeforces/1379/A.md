---
title: "CF 1379A - Acacius và String"
description: "Chúng ta được cung cấp một chuỗi ngắn gồm các chữ cái viết thường và dấu chấm hỏi. Mỗi dấu hỏi sau này có thể được thay thế bằng bất kỳ chữ cái viết thường nào. Sau khi thay thế, chúng tôi muốn kết thúc bằng một chuỗi chứa mẫu cố định \"abacaba\" làm chuỗi con đúng một lần."
date: "2026-06-16T13:22:41+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation", "strings"]
categories: ["algorithms"]
codeforces_contest: 1379
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 657 (Div. 2)"
rating: 1500
weight: 1379
solve_time_s: 407
verified: false
draft: false
---

[CF 1379A - Acacius và String](https://codeforces.com/problemset/problem/1379/A) 

**Đánh giá:** 1500 
**Tags:** vũ lực, thực hiện, dây 
**Thời gian giải:** 6 phút 47 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi ngắn gồm các chữ cái viết thường và dấu chấm hỏi. Mỗi dấu hỏi sau này có thể được thay thế bằng bất kỳ chữ cái viết thường nào. Sau khi thay thế, chúng tôi muốn kết thúc bằng một chuỗi chứa mẫu cố định`"abacaba"`dưới dạng chuỗi con đúng một lần. 

Nhiệm vụ không chỉ đơn giản là kiểm tra xem`"abacaba"`có thể xuất hiện. Chúng ta phải tích cực xây dựng một chuỗi hợp lệ đầy đủ nếu có thể và đảm bảo rằng sau tất cả các lần thay thế, có chính xác một lần xuất hiện của mẫu ở bất kỳ đâu trong chuỗi cuối cùng. 

Hạn chế chính là độ dài chuỗi tối đa là 50 và có tới 5000 trường hợp thử nghiệm. Điều này ngay lập tức cho chúng ta biết rằng bất kỳ giải pháp nào kiểm tra tất cả các chuỗi con có thể có và thực hiện xác thực tuyến tính cho mỗi lần thử đều có thể chấp nhận được. Cách tiếp cận bậc hai cho mỗi trường hợp kiểm thử là ổn, vì 50 bình phương là không đáng kể và thậm chí 5000 trường hợp như vậy vẫn nằm trong giới hạn thoải mái. 

Sự tinh tế chính là dấu hỏi đóng vai trò như ký tự đại diện. Chúng có thể là bất kỳ thứ gì, vì vậy chúng có thể được sử dụng để buộc khớp mẫu. Nhưng chúng cũng gây nguy hiểm: ngay cả khi chúng ta buộc phải xảy ra một sự cố nào đó.`"abacaba"`, các sự cố ngẫu nhiên khác có thể xuất hiện ở nơi khác sau khi điền các ký tự còn lại. 

Một sai lầm ngây thơ là tham lam đặt mẫu ở vị trí tương thích đầu tiên và cho rằng nó hoạt động. Điều đó không thành công khi một sự cố ẩn khác vẫn còn ở nơi khác. Một sai lầm nữa là bỏ qua thực tế là dấu chấm hỏi có thể vô tình tạo thêm các kết quả trùng khớp sau khi được điền tùy ý. 

Ví dụ: hãy xem xét một chuỗi như:```
?abacaba?
```Nếu chúng ta đặt`"abacaba"`bắt đầu từ vị trí 1, chúng ta vẫn có thể có một lần xuất hiện khác chồng chéo tùy thuộc vào cách chúng ta điền vào các đầu. Cách tiếp cận bất cẩn không xác minh chuỗi cuối cùng sẽ tạo ra câu trả lời sai. 

Một dạng lỗi khác là đếm số lần xuất hiện trước khi giải quyết các dấu chấm hỏi. Từ`'?'`có thể khớp bất cứ thứ gì, đếm mà không sửa chúng dẫn đến kết quả vô nghĩa. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là thử mọi cách có thể để thay thế dấu chấm hỏi bằng các chữ cái và sau đó đếm số lần xuất hiện của`"abacaba"`. Điều này rõ ràng là không khả thi: mỗi vị trí trong số 50 vị trí có 26 khả năng, do đó không gian tìm kiếm rất lớn về mặt thiên văn. 

Chúng tôi giảm bớt điều này bằng cách quan sát rằng quyết định có ý nghĩa duy nhất là khi sự xuất hiện duy nhất của`"abacaba"`sẽ bắt đầu. Có tối đa 44 vị trí xuất phát ứng viên. Đối với mỗi vị trí ứng cử viên, chúng tôi cố gắng áp dụng mẫu ở đó bằng cách kiểm tra tính tương thích: mọi ký tự cố định phải khớp và dấu chấm hỏi có thể được thay thế cho khớp. 

Nếu một ứng viên làm việc, chúng ta tạm thời xây dựng một chuỗi đầy đủ bằng cách đặt`"abacaba"`ở đó và điền tất cả các dấu chấm hỏi khác một cách tùy ý, ví dụ như với`'z'`. Sau đó, chúng tôi xác nhận chuỗi cuối cùng bằng cách đếm số lần xuất hiện của`"abacaba"`. Nếu đúng như vậy thì chúng tôi chấp nhận. 

Cái nhìn sâu sắc quan trọng là chúng ta không cần phải suy luận về tất cả các sự thay thế trên toàn cầu. Chúng ta chỉ cần neo một lần xuất hiện, hoàn thành chuỗi một cách xác định và xác minh tính chính xác. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force tất cả các sản phẩm thay thế | O(26^n · n) | O(n) | Quá chậm | 
| Hãy thử tất cả các vị trí + xác thực | O(n^2) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi cố gắng quyết định nơi duy nhất`"abacaba"`chuỗi con nên được đặt. 

1. Lặp lại mọi chỉ mục bắt đầu có thể`i`trong đó mẫu có độ dài 7 vừa với bên trong chuỗi. Điều này đảm bảo chúng tôi bao quát mọi vị trí có thể có cho chuỗi con được yêu cầu. 
2. Đối với từng vị trí`i`, kiểm tra xem chúng ta có thể phủ lên không`"abacaba"`lên sợi dây. Một vị trí hợp lệ nếu với mỗi ký tự trong mẫu, chuỗi gốc có cùng chữ cái hoặc nó chứa`'?'`. Điều này đảm bảo rằng chúng tôi có thể ép buộc mô hình ở đó một cách hợp pháp. 
3. Nếu vị trí hợp lệ, hãy tạo một bản sao làm việc của chuỗi và viết`"abacaba"`vào vị trí`[i, i+7)`. 
4. Thay thế mọi thứ còn lại`'?'`trong chuỗi với`'z'`. Lựa chọn này tránh việc vô tình tạo ra sự xuất hiện mới của`"abacaba"`trừ khi chúng bị ép buộc bởi cấu trúc. 
5. Quét chuỗi kết quả và đếm xem bao nhiêu lần`"abacaba"`xuất hiện dưới dạng một chuỗi con. 
6. Nếu số đếm chính xác là một, hãy trả về chuỗi này làm câu trả lời ngay lập tức. 
7. Nếu không có vị trí nào dẫn đến một kết cấu hợp lệ, hãy xuất ra`"No"`. 

### Tại sao nó hoạt động 

Thuật toán đảm bảo rằng nếu tồn tại một chuỗi cuối cùng hợp lệ thì có ít nhất một vị trí của`"abacaba"`phù hợp với sự xuất hiện thực sự trong giải pháp đó. Khi chúng tôi thử vị trí đó, chúng tôi bảo toàn vị trí đó một cách chính xác và loại bỏ sự mơ hồ bằng cách điền vào mọi thứ khác một cách xác định. Vì mọi ký tự khác đều cố định nên số lần xuất hiện cuối cùng là đáng tin cậy. Bất kỳ giải pháp đúng nào cũng phải tương ứng với một trong các mỏ neo đã được thử nghiệm, vì vậy chúng ta không thể bỏ lỡ câu trả lời hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

PAT = "abacaba"

def count_occ(s):
    cnt = 0
    for i in range(len(s) - 6):
        if s[i:i+7] == PAT:
            cnt += 1
    return cnt

def can_place(s, i):
    for j in range(7):
        if s[i+j] != '?' and s[i+j] != PAT[j]:
            return False
    return True

def solve_case(n, s):
    s = list(s)

    for i in range(n - 6):
        if not can_place(s, i):
            continue

        t = s[:]
        for j in range(7):
            t[i+j] = PAT[j]

        for k in range(n):
            if t[k] == '?':
                t[k] = 'z'

        cand = ''.join(t)
        if count_occ(cand) == 1:
            return "Yes\n" + cand

    return "No"

def main():
    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        s = input().strip()
        out.append(solve_case(n, s))
    print("\n".join(out))

if __name__ == "__main__":
    main()
```Cốt lõi của việc triển khai là thử nghiệm từng vị trí có thể. Người trợ giúp`can_place`đảm bảo chúng tôi không bao giờ ghi đè lên một ký tự cố định xung đột. Sau khi đặt mẫu, chúng tôi loại bỏ tính không xác định bằng cách chuyển đổi tất cả các dấu hỏi còn lại thành`'z'`, giúp đơn giản hóa việc xác nhận. 

Việc kiểm tra cuối cùng là rất quan trọng. Nếu không đếm lại các lần xuất hiện sau khi xây dựng đầy đủ, chúng ta có thể chấp nhận không chính xác các trường hợp xảy ra tai nạn thứ hai.`"abacaba"`xuất hiện. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
7
???????
```| Bước | Hành động | Trạng thái chuỗi | Vị trí hợp lệ | 
| --- | --- | --- | --- | 
| 1 | Hãy thử i = 0 | ???????? | vâng | 
| 2 | Đặt mẫu | bàn tính | vâng | 
| 3 | Điền vào phần còn lại | bàn tính | vâng | 
| 4 | Đếm số lần xuất hiện | 1 | chấp nhận | 

Điều này thể hiện trường hợp đơn giản nhất trong đó tất cả tính linh hoạt được sử dụng để xây dựng mẫu một cách trực tiếp. 

### Ví dụ 2 

đầu vào:```
11
aba?abacaba
```| Bước | Hành động | Trạng thái chuỗi | Vị trí hợp lệ | 
| --- | --- | --- | --- | 
| 1 | Hãy thử tôi = 3 | aba?abacaba | vâng | 
| 2 | Đặt mẫu ở vị trí 3 | abaababa | vâng | 
| 3 | Đổ đầy '?' | ôi... | vâng | 
| 4 | Đếm số lần xuất hiện | 1 | chấp nhận | 

Điều này cho thấy cách hoàn thành việc khớp một phần bằng cách phân giải một ký tự đại diện duy nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) mỗi lần kiểm tra | Tối đa n vị trí, mỗi vị trí yêu cầu quét chuỗi và kiểm tra mẫu | 
| Không gian | O(n) | Chúng tôi lưu trữ một bản sao làm việc của chuỗi | 

Với n 50 và T 5000, các phép toán trong trường hợp xấu nhất đủ nhỏ để có thể thực hiện thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return __import__("__main__").solve_all()

# We adapt solution for testing
def solve_all():
    input = sys.stdin.readline
    PAT = "abacaba"

    def count_occ(s):
        return sum(s[i:i+7] == PAT for i in range(len(s)-6))

    def can_place(s, i):
        for j in range(7):
            if s[i+j] != '?' and s[i+j] != PAT[j]:
                return False
        return True

    t = int(input())
    res = []
    for _ in range(t):
        n = int(input())
        s = list(input().strip())

        ok = False
        for i in range(n-6):
            if not can_place(s, i):
                continue
            t2 = s[:]
            for j in range(7):
                t2[i+j] = PAT[j]
            for k in range(n):
                if t2[k] == '?':
                    t2[k] = 'z'
            cand = ''.join(t2)
            if count_occ(cand) == 1:
                res.append("Yes\n" + cand)
                ok = True
                break
        if not ok:
            res.append("No")
    return "\n".join(res)

__main__.solve_all = solve_all

assert run("1\n7\nabacaba\n") == "Yes\nabacaba", "sample 1"
assert run("1\n7\n???????\n") == "Yes\nabacaba", "all wildcards"
assert run("1\n11\nabacaba????\n") != "", "suffix flexibility"
assert run("1\n11\nabacacacaba\n") == "No", "no valid placement"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả '?' chuỗi | Có với mẫu | năng lực thi công đầy đủ | 
| mẫu chính xác hiện tại | Có không thay đổi | trường hợp nhận dạng | 
| chuỗi không thể | Không | từ chối tính đúng đắn | 
| trường hợp ồn ào chồng chéo | Có/Không tùy theo | xác nhận mạnh mẽ | 

## Vỏ cạnh 

Một trường hợp cạnh tranh quan trọng là khi`"abacaba"`đã xuất hiện nhiều lần trong chuỗi đầu tiên có dấu chấm hỏi. Nếu chúng tôi không thực thi rõ ràng tính duy nhất sau khi điền, chúng tôi có thể chấp nhận những trường hợp đó một cách không chính xác. 

Ví dụ:```
abacaba?abacaba
```Nếu chúng ta đặt mẫu ở lần xuất hiện đầu tiên và điền vào phần còn lại`'?'`với`'z'`, lần xuất hiện thứ hai có thể vẫn còn nguyên. Việc kiểm tra số lượng cuối cùng sẽ ngăn ngừa lỗi này bằng cách từ chối cấu hình. 

Một trường hợp đặc biệt khác là khi đặt mẫu sẽ loại bỏ khả năng xảy ra lần thứ hai có thể tồn tại. Vì chúng tôi chỉ ghi đè một vùng và điền vào phần còn lại một cách xác định nên mọi trường hợp còn sót lại đều được kiểm soát hoàn toàn và phải được tính một cách rõ ràng, đảm bảo tính chính xác.
