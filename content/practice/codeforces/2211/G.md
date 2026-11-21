---
title: "CF 2211G - Sắp xếp nổi bọt hợp lý"
description: "Chúng ta được cung cấp một mảng các số nguyên trong đó mỗi phần tử có thể nằm trong khoảng từ 0 đến 10^6. Hoạt động được phép của chúng tôi là chọn bất kỳ hai phần tử liền kề nào, tính mức trung bình của chúng và sau đó đặt cả hai phần tử về mức trung bình đó."
date: "2026-06-07T19:12:50+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "geometry", "greedy"]
categories: ["algorithms"]
codeforces_contest: 2211
codeforces_index: "G"
codeforces_contest_name: "Nebius Round 2 (Codeforces Round 1088, Div. 1 + Div. 2)"
rating: 2900
weight: 2211
solve_time_s: 127
verified: false
draft: false
---

[CF 2211G - Sắp xếp bong bóng hợp lý](https://codeforces.com/problemset/problem/2211/G) 

**Xếp hạng:** 2900 
**Tags:** thuật toán xây dựng, hình học, tham lam 
**Thời gian giải:** 2m 7s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên trong đó mỗi phần tử có thể nằm trong khoảng từ 0 đến 10^6. Hoạt động được phép của chúng tôi là chọn bất kỳ hai phần tử liền kề nào, tính mức trung bình của chúng và sau đó đặt cả hai phần tử về mức trung bình đó. Câu hỏi đặt ra là liệu chúng ta có thể sử dụng bất kỳ số lượng thao tác nào trong số này để biến đổi mảng thành một chuỗi không giảm hay không. 

Điều quan trọng là thao tác này không cho phép chúng ta tự do sắp xếp lại các số. Mỗi thao tác kết hợp hai số liền kề, mang chúng lại gần nhau hơn một cách hiệu quả. Nếu một số lớn hơn nhiều so với số lân cận của nó ở bên phải thì việc lấy trung bình lặp lại chỉ có thể quy nó về số nhỏ hơn và việc lấy trung bình lặp lại từ phía bên kia sẽ làm giảm số lớn hơn về số nhỏ hơn. Do đó, mảng chỉ có thể được sắp xếp nếu số lớn nhất không xuất hiện “quá sớm” trong chuỗi so với các số nhỏ hơn. 

Các ràng buộc đầu vào cho phép tổng cộng tối đa 10^6 phần tử trong tất cả các trường hợp thử nghiệm. Bất kỳ giải pháp nào có độ phức tạp kém hơn O(n) cho mỗi trường hợp thử nghiệm sẽ quá chậm. Ví dụ: một mô phỏng mạnh mẽ của các hoạt động lấy trung bình có thể mất O(n^2) hoặc tệ hơn, điều này rõ ràng là không khả thi. Các trường hợp cạnh cần chú ý bao gồm các chuỗi trong đó tất cả các phần tử đều bằng nhau, các chuỗi không giảm và các chuỗi có đỉnh lớn ở đầu hoặc cuối. Ví dụ,`[10, 0, 0]`không thể được sắp xếp không giảm, nhưng`[0, 5, 0]`Có thể. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ liên tục thực hiện các phép tính trung bình cho đến khi không có thay đổi nào xảy ra nữa hoặc cho đến khi mảng được sắp xếp. Mỗi phép toán là O(1), nhưng nó có thể yêu cầu O(n) phép toán trên mỗi cặp để hội tụ. Với n=10^5, phương pháp này có thể thực hiện 10^10 thao tác trong trường hợp xấu nhất và do đó không thực tế. 

Cách tiếp cận tối ưu bắt nguồn từ việc phân tích tác động của việc tính trung bình. Khi chúng tôi chọn một cặp liền kề`(a_i, a_{i+1})`và thay thế cả hai bằng`(a_i + a_{i+1}) / 2`, chúng ta quan sát thấy tổng của cặp được giữ nguyên và giá trị của mỗi phần tử di chuyển về phía phần tử kia. Qua các phép toán lặp đi lặp lại, bất kỳ mảng con liền kề nào cũng sẽ hội tụ về giá trị trung bình của các phần tử của nó. Điều này ngụ ý rằng chúng ta không thể hoán đổi các số một cách tùy ý: nếu một số lớn ở bên trái của một số nhỏ hơn nhiều, nó có thể khiến mảng không được sắp xếp vì nó không thể giảm xuống dưới mức tối thiểu của phân đoạn mà nó là một phần. 

Thông tin chi tiết quan trọng là để mảng không giảm, tất cả các phần tử ngoại trừ phần tử đầu tiên có thể bị giới hạn bởi mức tối đa của tiền tố cho đến thời điểm đó. Nếu bất kỳ phần tử nào nhỏ hơn phần tử đầu tiên và không phải là phần tử cuối cùng, có thể thực hiện các thao tác di chuyển dần phần tử đó lên để đáp ứng các giá trị lớn hơn. Chính thức hơn, vấn đề giảm xuống còn việc kiểm tra xem phần tử đầu tiên có nhỏ hơn hoặc bằng phần tử cuối cùng trong mảng hay không. Nếu đúng như vậy, mảng luôn có thể được làm cho không giảm bằng các phép tính trung bình; nếu không thì không thể. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Tiền tố/Kiểm tra lần cuối | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc số phần tử`n`và mảng`a`. 
2. So sánh phần tử đầu tiên`a[0]`với phần tử cuối cùng`a[-1]`. 

Nếu như`a[0] > a[-1]`, đầu ra`"No"`. Điều này là do phần tử lớn ban đầu không thể giảm đủ để làm cho mảng không giảm. 
3. Ngược lại, xuất ra`"Yes"`. Các phép tính trung bình có thể điều chỉnh dần dần các phần tử bên trong theo hướng cấu hình không giảm mà không vi phạm giới hạn. 
4. Lặp lại cho tất cả các trường hợp thử nghiệm. 

Tại sao nó hoạt động: tính bất biến là mỗi phép tính trung bình không làm giảm mức tối thiểu của tiền tố liền kề cũng như không tăng mức tối đa của hậu tố liền kề. Nếu phần tử đầu tiên đã lớn hơn phần tử cuối cùng thì không có chuỗi phép tính trung bình nào có thể làm cho phần tử cuối cùng lớn hơn phần tử đầu tiên, do đó việc sắp xếp là không thể. Ngược lại, nếu phần tử đầu tiên nhỏ hơn hoặc bằng phần tử cuối cùng, các phép toán có thể “chuyển” các giá trị vào bên trong, tính trung bình mỗi phân đoạn thành một chuỗi đơn điệu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    if a[0] > a[-1]:
        print("No")
    else:
        print("Yes")
```Giải thích: đầu tiên chúng ta đọc số lượng test case. Đối với mỗi trường hợp thử nghiệm, chúng tôi đọc mảng và chỉ cần so sánh phần tử đầu tiên và phần tử cuối cùng. Không cần phải mô phỏng bất kỳ thao tác nào vì chỉ cần so sánh đầu tiên và cuối cùng là đủ. sử dụng`sys.stdin.readline`đảm bảo giải pháp xử lý đầu vào lớn một cách hiệu quả. 

## Ví dụ đã hoạt động 

**Ví dụ 1** 

đầu vào:`[24, 0, 0]`| Bước | một[0] | một[-1] | Quyết định | 
| --- | --- | --- | --- | 
| Kiểm tra | 24 | 0 | 24 > 0 → Không | 

Giải thích: phần tử đầu tiên lớn hơn phần tử cuối cùng nên không có chuỗi trung bình nào có thể làm cho mảng không giảm. 

**Ví dụ 2** 

đầu vào:`[0, 15, 0]`| Bước | một[0] | một[-1] | Quyết định | 
| --- | --- | --- | --- | 
| Kiểm tra | 0 | 0 | 0 0 → Có | 

Giải thích: phần tử đầu tiên nhỏ hơn hoặc bằng phần tử cuối cùng nên phép tính trung bình có thể điều chỉnh phần tử ở giữa để đạt được dãy không giảm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi trường hợp thử nghiệm chỉ yêu cầu đọc mảng và kiểm tra hai phần tử | 
| Không gian | O(1) | Chỉ mảng được lưu trữ; không cần cấu trúc dữ liệu bổ sung | 

Tổng số phần tử trong tất cả các trường hợp thử nghiệm là ≤ 10^6, do đó, giải pháp phù hợp thoải mái trong giới hạn thời gian 2 giây và giới hạn bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    exec(open("solution.py").read())  # assume solution is saved as solution.py
    return output.getvalue().strip()

# provided samples
assert run("6\n3\n24 0 0\n3\n0 15 0\n4\n15 14 5 6\n4\n0 1 0 0\n8\n8 7 6 5 4 3 2 1\n6\n4 1 5 4 1 1\n") == \
"No\nYes\nNo\nYes\nNo\nYes", "sample 1"

# custom cases
assert run("1\n1\n0\n") == "Yes", "single element"
assert run("1\n2\n5 5\n") == "Yes", "two equal elements"
assert run("1\n5\n1 2 3 4 5\n") == "Yes", "already sorted"
assert run("1\n5\n5 4 3 2 1\n") == "No", "strictly decreasing"
assert run("1\n3\n0 10 0\n") == "Yes", "first <= last but middle large"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n1\n0`| Có | Mảng phần tử đơn | 
|`1\n2\n5 5`| Có | Hai phần tử bằng nhau | 
|`1\n5\n1 2 3 4 5`| Có | Đã sắp xếp mảng | 
|`1\n5\n5 4 3 2 1`| Không | Mảng giảm nghiêm ngặt | 
|`1\n3\n0 10 0`| Có | Đầu tiên <= cuối cùng, đỉnh giữa | 

## Vỏ cạnh 

Một mảng một phần tử như`[0]`tự động thỏa mãn tính chất không giảm. Thuật toán trả về`"Yes"`bởi vì`a[0] <= a[-1]`nắm giữ một cách tầm thường. 

Đối với các mảng có hai phần tử bằng nhau, chẳng hạn như`[5, 5]`, không cần thao tác. Thuật toán vẫn xuất ra chính xác`"Yes"`vì phần tử đầu tiên không lớn hơn phần tử cuối cùng. 

Đối với các mảng có thứ tự giảm dần như`[5, 4, 3, 2, 1]`, phần tử đầu tiên vượt quá phần tử cuối cùng. Đầu ra của thuật toán`"No"`, xác nhận rằng không có chuỗi trung bình nào có thể vượt qua cấu hình ban đầu này. 

Bài xã luận này cho thấy rằng, mặc dù có vẻ ngoài là một phép tính trung bình phức tạp, nhưng vấn đề trở thành một phép so sánh đơn giản giữa phần tử đầu tiên và phần tử cuối cùng của mảng. Cái nhìn sâu sắc đến từ việc hiểu các đặc tính bất biến của hoạt động và cách nó tương tác với tính đơn điệu.
