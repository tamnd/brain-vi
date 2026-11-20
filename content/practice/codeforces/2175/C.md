---
title: "CF 2175C - Cây kim đáy bể"
description: "Chúng ta được cho hai chuỗi, s và t. Mục tiêu là sắp xếp lại các chữ cái của t sao cho s xuất hiện dưới dạng một chuỗi con trong chuỗi kết quả, đồng thời làm cho chuỗi cuối cùng về mặt từ điển càng nhỏ càng tốt."
date: "2026-06-09T04:32:19+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "strings"]
categories: ["algorithms"]
codeforces_contest: 2175
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 1069 (Div. 2)"
rating: 1200
weight: 2175
solve_time_s: 97
verified: false
draft: false
---

[CF 2175C - Cây kim trong đống cỏ khô](https://codeforces.com/problemset/problem/2175/C) 

**Đánh giá:** 1200 
**Tags:** tham lam, dây 
**Thời gian giải:** 1 phút 37 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho hai chuỗi,`s`Và`t`. Mục đích là sắp xếp lại các chữ cái của`t`như vậy`s`xuất hiện dưới dạng một chuỗi con trong chuỗi kết quả, đồng thời làm cho chuỗi cuối cùng về mặt từ điển càng nhỏ càng tốt. Một dãy con có nghĩa là chúng ta có thể xóa các chữ cái khỏi`t`nhưng không thể thay đổi thứ tự tương đối của chúng. Nếu không có cách nào để đưa vào`s`như một dãy con, chúng ta phải xuất ra "Không thể". 

Các ràng buộc chỉ ra rằng tổng độ dài của tất cả`t`chuỗi trong các trường hợp thử nghiệm tối đa là 100.000, do đó, bất kỳ giải pháp nào chạy theo thời gian tuyến tính đối với tổng kích thước đầu vào sẽ đủ hiệu quả. Độ phức tạp bậc hai hoặc cao hơn sẽ không phù hợp thoải mái dưới giới hạn 2 giây, vì`10^5 × 10^5`hoạt động là quá nhiều. 

Một trường hợp cạnh không rõ ràng phát sinh khi`t`chứa ít sự xuất hiện của một số ký tự hơn`s`nhu cầu. Ví dụ, nếu`s = "aa"`Và`t = "ab"`, chúng ta không thể hình thành`s`như một dãy con vì chỉ có một`'a'`TRONG`t`. Một trường hợp tinh tế khác là thứ tự từ điển: chỉ cần chèn`s`ở đầu hoặc cuối của việc sắp xếp`t`các chữ cái có thể không mang lại chuỗi nhỏ nhất. Ví dụ,`s = "dc"`Và`t = "abcd"`. Sắp xếp`t`cho`"abcd"`, nhưng chèn`s`lúc đầu tạo ra`"dcab"`, lớn hơn`"abcdc"`. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ xem xét tất cả các hoán vị của`t`và kiểm tra xem`s`là một dãy số. Điều này đúng về nguyên tắc nhưng hoàn toàn không khả thi: vì`|t| = 10^5`, số lượng hoán vị rất lớn về mặt thiên văn. Chúng tôi thậm chí không thể liệt kê chúng một phần. 

Quan sát quan trọng là vấn đề giảm xuống việc đếm các chữ cái và quyết định thứ tự của các chữ cái còn lại xung quanh`s`để duy trì sự tối thiểu về mặt từ điển. Cụ thể, nếu chúng ta biết mỗi ký tự xuất hiện bao nhiêu lần trong`t`, chúng ta có thể trừ số lượng cần thiết cho`s`và tự do sắp xếp lại các chữ cái còn lại. Thách thức chính là quyết định nơi đặt`s`trong số các chữ cái còn sót lại này để tạo ra kết quả nhỏ nhất về mặt từ điển. 

Một cái nhìn sâu sắc tham lam có tác dụng: hãy xem xét ký tự đầu tiên của`s`. Nếu chúng ta đặt`s`ở vị trí mà tất cả các chữ cái nhỏ hơn ký tự đầu tiên của`s`ở trước nó và tất cả các chữ cái lớn hơn ở sau nó, chúng ta thu được kết quả nhỏ nhất. Có một trường hợp tinh vi khi các chữ cái bằng ký tự đầu tiên của`s`xuất hiện trong phần còn lại: chúng ta cần quyết định xem chúng đi trước hay sau`s`. Sau khi kiểm tra các ví dụ, đặt chúng trước`s`chỉ khi chúng xảy ra trước vị trí đầu tiên trong`s`đảm bảo tối thiểu. Một chiến lược dễ dàng hơn là so sánh`s`với khối chữ giống nhau ở phần còn lại: nếu`s`về mặt từ điển nhỏ hơn một chuỗi các chữ cái giống nhau, đặt`s`Đầu tiên; nếu không thì đặt nó sau. 

Cách tiếp cận này hoạt động theo thời gian tuyến tính trên mỗi chuỗi vì chúng tôi chỉ đếm các chữ cái và xây dựng chuỗi cuối cùng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O( | t | !) | 
| Đếm Tham Lam | O( | t | ) | 

## Hướng dẫn thuật toán 

1. Đếm tần số của mỗi chữ cái trong`t`sử dụng một mảng có kích thước cố định gồm 26 phần tử, mỗi phần tử cho một chữ cái viết thường. Điều này cho phép chúng ta trừ nhanh các chữ cái được sử dụng trong`s`. 
2. Với mỗi chữ cái trong`s`, giảm số lượng của nó khỏi mảng tần số. Nếu bất kỳ số đếm nào âm, hãy in "Không thể" vì`t`không chứa đủ chữ cái để tạo thành`s`như một sự tiếp nối. 
3. Xây dựng chuỗi cuối cùng theo thứ tự. Đối với mỗi nhân vật`c`từ`'a'`ĐẾN`'z'`, nối thêm tất cả các bản sao còn lại của`c`trước hoặc sau`s`theo so sánh từ điển: 

- Nếu`c`nhỏ hơn ký tự đầu tiên của`s`, nối thêm tất cả còn lại`c`trước`s`. 
- Nếu như`c`bằng ký tự đầu tiên của`s`, so sánh chuỗi`s`với`c`lặp lại số còn sót lại`c`S. Nếu như`s`nhỏ hơn về mặt từ điển, hãy chèn`s`Đầu tiên; mặt khác, nối thêm phần còn lại`c`đầu tiên và sau đó`s`. 
- Nếu như`c`lớn hơn ký tự đầu tiên của`s`, nối thêm tất cả còn lại`c`sau đó`s`. 
4. Sau khi đặt tất cả các ký tự, chúng ta thu được chuỗi nhỏ nhất về mặt từ điển chứa`s`như một sự tiếp nối. 

Tại sao nó hoạt động: thuật toán đảm bảo tất cả các chữ cái nhỏ hơn phần đầu của`s`xuất hiện trước`s`và tất cả các chữ cái lớn hơn xuất hiện sau. Việc xử lý cẩn thận các chữ cái bằng nhau đảm bảo chúng ta không tạo ra một chuỗi lớn hơn mức cần thiết. Việc đếm đảm bảo chúng tôi không bao giờ xóa nhiều chữ cái hơn mức có sẵn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    T = int(input())
    for _ in range(T):
        s = input().strip()
        t = input().strip()
        freq = [0]*26
        for c in t:
            freq[ord(c)-ord('a')] += 1
        for c in s:
            freq[ord(c)-ord('a')] -= 1
            if freq[ord(c)-ord('a')] < 0:
                print("Impossible")
                break
        else:
            result = []
            first_s = s[0]
            for i in range(26):
                c = chr(i+ord('a'))
                if c < first_s:
                    result.append(c*freq[i])
                elif c == first_s:
                    # Decide whether s should go before or after leftover c's
                    leftover_c = c*freq[i]
                    if s < leftover_c:
                        result.append(s)
                        result.append(leftover_c)
                    else:
                        result.append(leftover_c)
                        result.append(s)
                    freq[i] = 0
                else:
                    result.append(c*freq[i])
            print(''.join(result))

if __name__ == "__main__":
    solve()
```Mã đầu tiên đếm tất cả các chữ cái trong`t`và loại bỏ những cái cần thiết cho`s`. Nếu tại bất kỳ thời điểm nào một lá thư trong`s`thiếu thì in ngay "Không thể". Ngược lại, chúng ta lặp qua tất cả các chữ cái, đặt các chữ cái nhỏ hơn chữ cái đầu tiên`s`trước nó. Các chữ cái bằng ký tự đầu tiên được xử lý cẩn thận bằng cách so sánh`s`với khối chữ cái còn sót lại để quyết định điểm chèn tối thiểu. Các chữ cái còn lại được thêm vào sau đó. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:`s = "dcbe"`,`t = "bedbaecfc"`| Bước | mảng tần số (một phần) | Chuỗi kết quả | 
| --- | --- | --- | 
| Đếm t | b:2, c:2, d:2, e:2, a:1, f:1 | "" | 
| Trừ s | d:1, c:1, b:1, e:1 | "" | 
| Xây dựng kết quả | a:1 → "a", b=1 → "b", c=1 → "c", d=1 → "d", e=1 → "e", f=1 → "f" | "abcdcbò" | 

Điều này tạo ra chuỗi nhỏ nhất về mặt từ điển dự kiến ​​có chứa`s`. 

Đầu vào mẫu 2:`s = "bab"`,`t = "ababa"`| Bước | mảng tần số (một phần) | Chuỗi kết quả | 
| --- | --- | --- | 
| Đếm t | a:3, b:2 | "" | 
| Trừ s | b:1, a:2, b:1 | "" | 
| Xây dựng kết quả | a=2 → "aa", b=1 → "b", chèn s="bab" | "aa" + "bab" + "b"? Thực ra "aa" + "bab" = "aabab" | 

Chứng tỏ cách các chữ cái còn sót lại trước đây`s`Và`s`vị trí tạo ra chuỗi tối thiểu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O( | t | 
| Không gian | O(1) | Mảng có kích thước cố định gồm 26 ký tự đủ | 

Cho tổng của`|t|`trên tất cả các trường hợp thử nghiệm là 10^5, thuật toán này chạy hiệu quả trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    solve()
    sys.stdout = sys.__stdout__
    return output.getvalue().strip()

# Provided samples
assert run("3\ndcbe\nbedbaecfc\nbabadab\nabacabadabacaba\nbabaisyou\nflagiswin") == "abcdcbeef\naaaaabababccdab\nImpossible"

# Custom cases
assert run("1\na\na") == "a", "single letter input"
assert run("1\nab\nba") == "ab", "reordering needed"
assert run("1\naa\naa") == "aa", "all letters identical"
assert run("1\nabc\ndefabc") == "abcdef", "s at end"
assert run("1\nabc\naabbcc") == "aabbccabc", "multiple same letters"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| một/một | một | đầu vào kích thước tối thiểu | 
| ab/ba | ab | sắp xếp lại đơn giản |
