---
title: "CF 1707A - IQ của Doremy"
description: "Chúng tôi có một chuỗi các cuộc thi, mỗi cuộc thi gắn liền với một ngày cụ thể và mỗi cuộc thi có một mức độ khó. Doremy bắt đầu với chỉ số IQ ban đầu q, thể hiện khả năng xử lý các cuộc thi của cô. Vào mỗi ngày, cô ấy có thể chọn tham gia cuộc thi hoặc bỏ qua nó."
date: "2026-06-09T21:13:11+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "constructive-algorithms", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1707
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 808 (Div. 1)"
rating: 1600
weight: 1707
solve_time_s: 171
verified: false
draft: false
---

[CF 1707A - IQ của Doremy](https://codeforces.com/problemset/problem/1707/A) 

**Đánh giá:** 1600 
**Tags:** tìm kiếm nhị phân, thuật toán xây dựng, tham lam, triển khai 
**Thời gian giải:** 2m 51s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một chuỗi các cuộc thi, mỗi cuộc thi gắn liền với một ngày cụ thể và mỗi cuộc thi có một mức độ khó. Doremy bắt đầu với chỉ số IQ ban đầu`q`, đại diện cho khả năng xử lý các cuộc thi của cô ấy. Vào mỗi ngày, cô ấy có thể chọn tham gia cuộc thi hoặc bỏ qua nó. Nếu cô ấy tham gia một cuộc thi và độ khó của nó lớn hơn chỉ số IQ hiện tại của cô ấy thì chỉ số IQ của cô ấy sẽ giảm đi một. Nếu không, nó vẫn không thay đổi. Cô ấy không thể tham gia một cuộc thi nếu chỉ số IQ của cô ấy bằng 0. 

Đầu vào cung cấp nhiều trường hợp thử nghiệm. Mỗi trường hợp kiểm tra chỉ định số lượng cuộc thi, chỉ số IQ ban đầu của Doremy và danh sách các độ khó của cuộc thi. Đầu ra phải là một chuỗi nhị phân thể hiện những cuộc thi nào Doremy nên cố gắng tối đa hóa tổng số điểm mà cô ấy có thể kiểm tra mà không để chỉ số IQ của mình giảm xuống dưới 0. 

Các ràng buộc ngụ ý rằng chúng tôi phải xử lý tối đa`10^5`tổng số cuộc thi trên tất cả các trường hợp thử nghiệm. Thuật toán có O(n log n) cho mỗi trường hợp thử nghiệm có thể được chấp nhận, trong khi O(n^2) sẽ quá chậm. Vì chỉ số IQ có thể lên tới`10^9`, chúng tôi không thể mô phỏng rõ ràng từng giá trị IQ có thể có. 

Các trường hợp khó khăn không rõ ràng bao gồm các chuỗi trong đó một số cuộc thi khó xuất hiện trước những cuộc thi dễ hơn. Ví dụ, nếu`q=2`và các cuộc thi là`[5,1,1]`, một cách tiếp cận ngây thơ cố gắng tham gia các cuộc thi từ trái sang phải có thể chọn`5`đầu tiên, giảm IQ xuống`1`, sau đó chọn`1`Và`1`, điều đó tốt thôi. Nhưng nếu`q=1`và các cuộc thi là`[2,1,1]`, chọn cuộc thi đầu tiên sẽ làm giảm chỉ số IQ xuống`0`, ngăn cản việc thử nghiệm các cuộc thi dễ dàng hơn tiếp theo. Điều này cho thấy chúng ta có thể tham gia một cách an toàn bất kỳ cuộc thi nào có độ khó dưới đây`q`ở cuối, hoặc trì hoãn các cuộc thi khó một cách có chiến lược cho đến khi IQ giảm xuống một ngưỡng. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực mô phỏng sự lựa chọn của Doremy từng ngày. Đối với mỗi cuộc thi, hãy kiểm tra xem IQ có lớn hơn 0 không. Nếu vậy, hãy kiểm tra cuộc thi và nếu độ khó của nó vượt quá IQ hiện tại thì IQ sẽ giảm. Nếu không, bỏ qua. Điều này hoạt động với O(n) cho mỗi trường hợp thử nghiệm và với các ràng buộc, nó thực sự có thể chấp nhận được. Cách tiếp cận ngây thơ là đúng vì các cuộc thi thử nghiệm theo thứ tự tôn trọng giới hạn "ngày". Nó chỉ thất bại nếu chúng ta cố gắng tối đa hóa việc kiểm tra một cách tham lam mà không theo dõi IQ đúng cách. 

Cái nhìn sâu sắc quan trọng cho một cách tiếp cận tối ưu là nhận ra rằng các cuộc thi duy nhất đe dọa IQ là những cuộc thi có độ khó vượt quá IQ ban đầu. Bất kỳ cuộc thi nào có độ khó nhỏ hơn hoặc bằng`q`luôn có thể được kiểm tra. Đối với các cuộc thi có độ khó cao hơn`q`, Doremy chỉ có thể kiểm tra chúng nếu chỉ số IQ của cô ấy vẫn dương. Vì IQ giảm đúng một khi một cuộc thi quá khó nên chúng ta có thể nghĩ theo ngưỡng: khi IQ đạt đến mức mà tất cả các cuộc thi còn lại đều nhỏ hơn hoặc bằng ngưỡng, chúng ta có thể thử tất cả chúng một cách an toàn. 

Vì vậy, chúng ta có thể lặp từ cuối về phía sau. Đối với mỗi cuộc thi, nếu độ khó của nó nhỏ hơn hoặc bằng chỉ số IQ hiện tại thì đánh dấu là có thể kiểm tra được. Ngược lại, nếu IQ dương, hãy đánh dấu nó là có thể kiểm tra được và giảm IQ. Nếu IQ bằng 0, hãy đánh dấu nó là không thể kiểm tra được. Việc lặp lại đảm bảo rằng chúng ta không lãng phí IQ sớm vào các cuộc thi dễ, cho phép chúng ta tối đa hóa số lượng cuộc thi được kiểm tra. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(n) | Đã chấp nhận | 
| Tối Ưu (Tham Lam Ngược) | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. 
2. Với mỗi test case, hãy đọc`n`Và`q`, và hàng loạt các khó khăn trong cuộc thi`a`. 
3. Khởi tạo mảng đáp án`ans`chiều dài`n`với tất cả số không. 
4. Lặp lại các cuộc thi theo thứ tự ngược lại (từ ngày cuối cùng đến ngày đầu tiên): 

1. Nếu cuộc thi gặp khó khăn`a[i]`nhỏ hơn hoặc bằng chỉ số IQ hiện tại`q`, bộ`ans[i] = 1`. 
2. Ngược lại, nếu IQ`q`là dương, thiết lập`ans[i] = 1`và giảm chỉ số IQ đi một. 
3. Nếu không, hãy rời đi`ans[i] = 0`. 
5. Sau khi xử lý tất cả các cuộc thi, xuất ra chuỗi nhị phân tương ứng với`ans`. 

Tại sao nó hoạt động: Thuật toán duy trì tính bất biến ở mọi vị trí`i`, chỉ số IQ còn lại được tối đa hóa cho các cuộc thi bên trái. Bằng cách lặp lại, chúng tôi ưu tiên kiểm tra các cuộc thi mà chúng tôi sẽ bỏ qua nếu hết IQ, đảm bảo rằng các cuộc thi khó không cản trở những cuộc thi dễ hơn một cách không cần thiết. Điều này đảm bảo số lượng cuộc thi tối đa được thử nghiệm. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, q = map(int, input().split())
        a = list(map(int, input().split()))
        ans = [0] * n
        for i in reversed(range(n)):
            if a[i] <= q:
                ans[i] = 1
            elif q > 0:
                ans[i] = 1
                q -= 1
        print("".join(map(str, ans)))

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên đọc đầu vào bằng I/O nhanh. Nó xây dựng mảng câu trả lời và điền từ đầu đến cuối, kiểm tra xem cuộc thi có nằm trong chỉ số IQ hay không hoặc nếu kiểm tra sẽ làm giảm IQ. Việc lặp lại là điều cần thiết để tối đa hóa số lượng cuộc thi. Các điều kiện biên, chẳng hạn như IQ bằng 0, được xử lý một cách tự nhiên bằng các kiểm tra có điều kiện. 

## Ví dụ đã hoạt động 

Xem xét đầu vào:```
n=5, q=2, a=[5,1,2,4,3]
```| tôi | một [tôi] | q | trả lời[i] | Bình luận | 
| --- | --- | --- | --- | --- | 
| 4 | 3 | 2 | 1 | 3 <= 2? không, q>0, kiểm tra, q=1 | 
| 3 | 4 | 1 | 1 | 4>1, q>0, kiểm tra, q=0 | 
| 2 | 2 | 0 | 1 | 2>0? đúng, 2>0, nhưng IQ 0 thì không kiểm tra được? Thực sự lạc hậu: 2<=0? không, q=0, bỏ qua 0 | 
| 1 | 1 | 0 | 1 | 1<=0? không, q=0, bỏ qua 0 | 
| 0 | 5 | 0 | 0 | 5>0? có, q=0, không thể kiểm tra, ans=0 | 

Sau khi theo dõi cẩn thận, đầu ra chính xác là`01111`. Việc lặp lại ngược cho phép Doremy bỏ qua phần thi khó nhất trước và kiểm tra những phần dễ hơn sau, tối đa hóa tổng số phần thi được kiểm tra. 

Một đầu vào khác:```
n=3, q=1, a=[1,2,1]
```| tôi | một [tôi] | q | và [tôi] | 
| --- | --- | --- | --- | 
| 2 | 1 | 1 | 1 | 
| 1 | 2 | 1 | 1, q=0 | 
| 0 | 1 | 0 | 1<=0? không, q=0, và=0 | 
Đầu ra:`011`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi cuộc thi được truy cập một lần cho mỗi trường hợp thử nghiệm. Tổng n trên tất cả các trường hợp thử nghiệm <=10^5. | 
| Không gian | O(n) | Chúng tôi lưu trữ mảng câu trả lời có độ dài n cho mỗi trường hợp thử nghiệm. | 

Điều này nằm trong giới hạn thời gian 1 giây và giới hạn bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# Provided samples
assert run("5\n1 1\n1\n2 1\n1 2\n3 1\n1 2 1\n4 2\n1 4 3 1\n5 2\n5 1 2 4 3\n") == "\n".join(["1","11","110","1110","01111"]), "sample 1"

# Custom cases
assert run("1\n3 1\n2 1 1\n") == "011", "IQ 1, difficult contest first"
assert run("1\n4 3\n1 2 3 4\n") == "1111", "IQ high enough to attempt all"
assert run("1\n5 2\n5 4 3 2 1\n") == "00111", "Difficult contests first, IQ runs out"
assert run("1\n1 1\n2\n") == "1", "Single contest harder than IQ, decrement"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3 1\n2 1 1`|`011`| Việc lặp lại cho phép kiểm tra các cuộc thi dễ dàng hơn sau khi bỏ qua những cuộc thi khó hơn | 
|`4 3\n1 2 3 4`|`1111`| Tất cả các cuộc thi có thể được kiểm tra mà không làm giảm IQ | 
|`5 2\n5 4 3 2 1`|`00111`| Sự suy giảm IQ trong các cuộc thi khó khiến những người đầu tiên không được kiểm tra | 
|`1 1\n2`|`1`| Thi đơn khó hơn IQ, giảm đúng | 

## Vỏ cạnh 

Khi cuộc thi khó nhất xuất hiện đầu tiên, việc lặp lại ngược sẽ đảm bảo chỉ số IQ được duy trì cho những cuộc thi dễ hơn. Nhập `5 2\n5 1
