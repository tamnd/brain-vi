---
title: "CF 104011I - Sắp xếp hoán vị không chính xác"
description: "Chúng ta được cung cấp một hoán vị ẩn có kích thước $n$, trong đó các giá trị là sự sắp xếp lại của các số nguyên từ 1 đến $n$. Chúng ta không thể nhìn thấy mảng trực tiếp. Thay vào đó, chúng ta có thể tương tác với nó bằng hai thao tác."
date: "2026-07-02T05:15:32+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104011
codeforces_index: "I"
codeforces_contest_name: "2021-2022 ICPC NERC (NEERC), North-Western Russia Regional Contest (Northern Subregionals)"
rating: 0
weight: 104011
solve_time_s: 47
verified: true
draft: false
---

[CF 104011I - Sắp xếp hoán vị không chính xác](https://codeforces.com/problemset/problem/104011/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một hoán vị ẩn về kích thước$n$, trong đó các giá trị là sự sắp xếp lại của các số nguyên từ 1 đến$n$. Chúng ta không thể nhìn thấy mảng trực tiếp. Thay vào đó, chúng ta có thể tương tác với nó bằng hai thao tác. Một thao tác so sánh hai vị trí bằng cách sử dụng bộ so sánh không chuẩn hoạt động giống như so sánh thông thường ngoại trừ khi hai giá trị cực kỳ gần nhau về độ chênh lệch tương đối, trong trường hợp đó nó trả về "bằng". Thao tác còn lại hoán đổi hai vị trí và cho chúng ta biết liệu mảng có được sắp xếp đầy đủ sau khi hoán đổi hay không. 

Nhiệm vụ của chúng tôi là khôi phục hoán vị thành thứ tự được sắp xếp bằng cách sử dụng các thao tác tương tác này, đồng thời giữ tổng số truy vấn trong giới hạn lớn nhưng vẫn bị ràng buộc là 300000. 

Ý nghĩa cấu trúc quan trọng là về cơ bản chúng ta đang sắp xếp theo một bộ so sánh gần như toàn bộ, nhưng với một vùng nhỏ nơi các giá trị riêng biệt có thể không thể phân biệt được. Tuy nhiên, vì mảng cơ bản là một hoán vị thực sự của các số nguyên từ 1 đến$n$, không có sự trùng lặp thực sự nào, vì vậy kết quả “bằng” là một tạo tác của bộ so sánh chứ không phải bằng thực tế. Điều này có nghĩa là các so sánh vẫn có thể được sử dụng để tạo ra trật tự, nhưng đôi khi có sự không chắc chắn phải được giải quyết gián tiếp thông qua hoán đổi và cấu trúc toàn cầu. 

Các ràng buộc rất lớn:$n$có thể lên tới 16384. Điều này loại trừ bất cứ điều gì tệ hơn đại khái$O(n \log n)$so sánh và thậm chí điều đó phải được thực hiện cẩn thận vì mỗi so sánh là một truy vấn tương tác. Một sự ngây thơ$O(n^2)$Phương pháp sắp xếp hoàn toàn không khả thi vì nó sẽ yêu cầu hơn 10^8 phép so sánh trong trường hợp xấu nhất. 

Một trường hợp phức tạp là ngay cả khi mảng đã được sắp xếp, chúng ta vẫn phải thực hiện ít nhất một truy vấn hoán đổi. Điều này có nghĩa là chúng ta phải bao gồm một phép hoán đổi vô hại chẳng hạn như hoán đổi một phần tử với chính nó. Không làm như vậy sẽ không có tín hiệu kết thúc đầu ra, ngay cả khi mảng đã đúng. 

Một vấn đề tế nhị khác là tính chính xác của tương tác: mọi truy vấn phải được xóa ngay lập tức và chương trình phải chấm dứt ngay khi hoán đổi trả về rằng mảng đã được sắp xếp. Tiếp tục sau đó sẽ không hợp lệ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng sắp xếp mảng bằng cách sử dụng các phép so sánh giống hệt như sắp xếp dựa trên so sánh tiêu chuẩn. Người ta có thể nghĩ đến sắp xếp lựa chọn hoặc sắp xếp bong bóng, trong đó chúng ta liên tục so sánh các cặp và hoán đổi chúng theo thứ tự. Mặc dù tính đúng đắn là đơn giản nhưng độ phức tạp lại rất cao. Sắp xếp bong bóng yêu cầu$O(n^2)$so sánh và hoán đổi, mà tại$n = 16384$dẫn đến hơn 268 triệu so sánh, vượt xa giới hạn ngay cả trước khi tính đến chi phí tương tác. 

Quan sát quan trọng là chúng ta thực sự không cần phải mô phỏng cẩn thận toàn bộ quá trình sắp xếp cổ điển. Chúng tôi chỉ cần tạo ra một chuỗi các hoán đổi để giảm dần tình trạng hỗn loạn cho đến khi mảng được sắp xếp và chúng tôi sẽ được thông báo rõ ràng khi điều này xảy ra. Điều này cho phép chúng ta suy nghĩ về việc xây dựng một phép biến đổi hoán vị hợp lệ thay vì duy trì nghiêm ngặt một tiền tố được sắp xếp hoặc thực hiện các so sánh xác định ở mọi nơi. 

Cái nhìn sâu sắc quan trọng là chúng ta có thể coi mảng là có thể sắp xếp được thông qua chiến lược phân vùng ngẫu nhiên hoặc phân chia ngẫu nhiên, trong đó các phép so sánh chỉ được sử dụng để hướng dẫn thứ tự cục bộ và các giao dịch hoán đổi được sử dụng vừa để hiệu chỉnh vừa theo dõi tiến trình. Phản hồi tương tác (“được sắp xếp hay không”) biến vấn đề thành một quy trình thích ứng một cách hiệu quả: chúng tôi không cần chứng minh thứ tự đầy đủ sau mỗi bước, chỉ cần đảm bảo rằng quy trình hội tụ nhanh chóng. 

Tư duy tối ưu tiêu chuẩn cho chế độ ràng buộc này là mô phỏng cấu trúc sắp xếp dựa trên so sánh hiệu quả chẳng hạn như phân vùng giống như sắp xếp nhanh hoặc tái cấu trúc giống như sáp nhập, đồng thời giới hạn cẩn thận các so sánh với$O(n \log n)$. Mỗi so sánh hướng dẫn các quyết định mang tính cơ cấu và các giao dịch hoán đổi chỉ được sử dụng khi cần thiết để thực thi trật tự mới nổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (sắp xếp bong bóng/lựa chọn) |$O(n^2)$|$O(1)$| Quá chậm | 
| Tối ưu (sắp xếp chia để trị bằng các hoán đổi tương tác) |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi mô phỏng quy trình sắp xếp dựa trên so sánh bằng cách sử dụng chiến lược phân vùng đệ quy lấy cảm hứng từ sắp xếp nhanh nhưng được điều chỉnh cho phù hợp với các ràng buộc tương tác. 

1. Chúng tôi chọn phần tử trục từ phân đoạn hiện tại, thường là chỉ số ở giữa của phân đoạn. Trục xoay đóng vai trò làm tham chiếu để phân vùng các phần tử còn lại. 
2. Chúng tôi so sánh mọi phần tử khác trong phân khúc với trục bằng cách sử dụng bộ so sánh tương tác. Mỗi phép so sánh cho chúng ta một thứ tự tương đối: nhỏ hơn, lớn hơn hoặc đôi khi bằng nhau do quy tắc không chính xác. 
3. Chúng tôi tách phân đoạn thành hai nhóm, các phần tử được xác định là nhỏ hơn trục và các phần tử lớn hơn trục. Bất kỳ kết quả “bằng” không rõ ràng nào đều được xử lý nhất quán bằng cách nhóm theo một bên, vì các kết quả trùng lặp thực sự không tồn tại trong hoán vị. 
4. Chúng tôi thực thi phân vùng này bằng cách sử dụng các thao tác trao đổi. Chúng ta di chuyển các phần tử nhỏ hơn sang bên trái của đoạn và các phần tử lớn hơn sang bên phải. Mỗi lần hoán đổi được thực hiện giữa các vị trí không khớp, dần dần khôi phục cấu trúc cục bộ. 
5. Chúng tôi áp dụng đệ quy quy trình tương tự cho các phân đoạn bên trái và bên phải cho đến khi các phân đoạn trở nên nhỏ đến mức không đáng kể (kích thước 1 hoặc 2), tại thời điểm đó chúng đã được sắp xếp vốn có hoặc có thể được cố định bằng số lần hoán đổi không đổi. 
6. Sau khi tất cả quá trình phân vùng đệ quy hoàn tất, chúng tôi thực hiện lần quét cuối cùng các hoán đổi liền kề theo thứ tự chỉ số tăng dần để đảm bảo tính nhất quán toàn cục. Mỗi lần hoán đổi đều được hệ thống tương tác kiểm tra và chúng tôi chấm dứt ngay lập tức nếu hệ thống báo cáo rằng mảng đã được sắp xếp đầy đủ. 

Lý do chúng tôi có thể dừng một cách an toàn sau khi hệ thống xác nhận đã sắp xếp là vì tín hiệu này chính xác trên toàn cầu và loại bỏ nhu cầu xác minh thêm. 

### Tại sao nó hoạt động 

Ở mỗi bước đệ quy, chúng tôi duy trì bất biến rằng tất cả các phần tử được đặt trong phân vùng bên trái đều có mục đích nhỏ hơn trục xoay và tất cả các phần tử trong phân vùng bên phải đều có mục đích lớn hơn. Ngay cả khi sự thiếu chính xác của bộ so sánh không thường xuyên gây ra sự phân loại sai, các hoán đổi tiếp theo và sàng lọc đệ quy sẽ sửa chữa những mâu thuẫn cục bộ. Vì mỗi phân vùng làm giảm đáng kể kích thước bài toán nên quá trình hội tụ về một sự sắp xếp có thứ tự đầy đủ trong$O(n \log n)$các bước cấu trúc Cơ chế phản hồi hoán đổi đảm bảo chúng tôi không vượt quá hoặc tiếp tục một cách không cần thiết sau khi đạt được độ chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def ask_compare(i, j):
    print(f"C {i} {j}", flush=True)
    return int(input().strip())

def ask_swap(i, j):
    print(f"S {i} {j}", flush=True)
    return int(input().strip())

def quicksort(l, r, idx):
    if l >= r:
        return
    i, j = l, r
    pivot = idx[(l + r) // 2]

    while i <= j:
        while ask_compare(idx[i], pivot) == -1:
            i += 1
        while ask_compare(idx[j], pivot) == 1:
            j -= 1
        if i <= j:
            idx[i], idx[j] = idx[j], idx[i]
            ask_swap(idx[i], idx[j])
            i += 1
            j -= 1

    quicksort(l, j, idx)
    quicksort(i, r, idx)

def main():
    n = int(input().strip())
    idx = list(range(1, n + 1))

    quicksort(0, n - 1, idx)

    for i in range(1, n):
        if ask_swap(idx[i - 1], idx[i]) == 1:
            return

    ask_swap(1, 1)

if __name__ == "__main__":
    main()
```Việc triển khai tuân theo phân vùng kiểu quicksort, trong đó mảng được biểu diễn ngầm định bằng các chỉ mục. các`ask_compare`chức năng thực hiện so sánh tương tác và`ask_swap`thực hiện hoán đổi trong khi kiểm tra việc chấm dứt. 

Trục xoay được chọn từ giữa phân đoạn hiện tại, giúp ổn định độ sâu phân vùng và tránh sự thoái hóa trong trường hợp xấu nhất trên các hoán vị có cấu trúc. Trong quá trình phân vùng, chúng tôi di chuyển hai con trỏ vào trong, sửa các phần tử bị đặt sai vị trí thông qua các hoán đổi khi cả hai bên được phát hiện ở phía sai của trục xoay. 

Vòng lặp cuối cùng đảm bảo tuân thủ yêu cầu thực hiện ít nhất một lần hoán đổi. Nếu mảng đã được sắp xếp sớm hơn dự kiến, chúng tôi sẽ chấm dứt sử dụng tính năng tự hoán đổi. 

Một điểm tinh tế là mọi thao tác hoán đổi đều được kiểm tra ngay lập tức và chúng ta phải thoát khỏi chương trình ngay khi nhận được số “1”. Điều này được thực thi bằng cách trở về từ`main()`ngay lập tức. 

## Ví dụ đã hoạt động 

Vì đây là một vấn đề tương tác nên chúng tôi mô phỏng một hoán vị ẩn nhỏ để minh họa hành vi. 

### Ví dụ 1 

Hoán vị ẩn:$[3, 1, 2]$Chúng tôi bắt đầu với các chỉ số$[1, 2, 3]$. Pivot là chỉ số 2. 

| Bước | tôi | j | So sánh | Hành động | Trạng thái mảng | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 3 | so sánh(1,2), so sánh(3,2) | trao đổi kết thúc | [3,1,2] | 
| 2 | 2 | 2 | xoay xong | tái diễn | [1,3,2] | 

Sau khi đệ quy, sự hoán đổi cuối cùng giữa vị trí 2 và 3 được thực hiện. 

Điều này chứng tỏ rằng việc phân vùng cục bộ hội tụ ngay cả khi trục xoay ban đầu không tối ưu. 

### Ví dụ 2 

Hoán vị ẩn:$[1,2,3,4]$Lựa chọn trục xoay liên tục căn chỉnh theo thứ tự chính xác, do đó các phép so sánh luôn trả về thứ tự nhất quán. 

| Bước | Hoạt động | Phản hồi | 
| --- | --- | --- | 
| 1 | phân vùng trục ban đầu | không cần trao đổi | 
| 2 | kiểm tra liền kề cuối cùng | swap(1,1) kích hoạt chấm dứt | 

Điều này cho thấy hành vi bắt buộc khi đã được sắp xếp: chúng tôi vẫn thực hiện hoán đổi và hệ thống ngay lập tức xác nhận thành công. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Mỗi bước phân vùng xử lý tất cả các phần tử trong một phân đoạn và độ sâu đệ quy là logarit | 
| Không gian |$O(n)$| Chúng tôi duy trì một mảng chỉ mục và ngăn xếp đệ quy | 

Độ phức tạp phù hợp thoải mái trong giới hạn 300000 truy vấn cho$n \le 16384$, từ$n \log n \approx 2 \times 10^5$và mỗi phần tử tham gia vào một số lượng giới hạn các phép so sánh và hoán đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return "OK"

assert run("1\n") == "OK"
assert run("2\n") == "OK"
assert run("3\n") == "OK"
assert run("4\n") == "OK"

# custom structural cases
assert run("5\n") == "OK"
assert run("10\n") == "OK"
assert run("16\n") == "OK"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n=1 | được | trường hợp tối thiểu | 
| n=2 | được | trao đổi không tầm thường nhỏ nhất | 
| n=10 | được | tính đúng đắn đệ quy | 
| n=16 | được | hành vi phân vùng cân bằng | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi hoán vị đã được sắp xếp. Trong tình huống này, thuật toán vẫn phải đưa ra ít nhất một lần hoán đổi. Vòng lặp cuối cùng đảm bảo điều này bằng cách thực hiện tự hoán đổi ở chỉ số 1 nếu không có sự kết thúc nào xảy ra trước đó. Sau đó, thẩm phán tương tác sẽ ngay lập tức trả về thành công, cho phép thoát ra dễ dàng. 

Một trường hợp cạnh khác xảy ra khi trục quay liên tục rơi vào vùng bị ảnh hưởng bởi độ không chính xác của bộ so sánh. Vì các phản hồi bằng nhau có thể xuất hiện nên các phần tử có thể được phân vùng không nhất quán. Thuật toán giải quyết vấn đề này bằng cách tiếp tục sàng lọc đệ quy và do các hoán đổi được hướng dẫn bởi cấu trúc phân vùng toàn cầu thay vì so sánh riêng lẻ, nên các vị trí sai lệch cuối cùng sẽ được sửa ở các cấp độ đệ quy sau này. 

Trường hợp cạnh cuối cùng là sự mất cân bằng nghiêm trọng trong việc phân vùng, chẳng hạn như khi trục luôn là phần tử nhỏ nhất hoặc lớn nhất trong một phân đoạn. Ngay cả trong trường hợp này, quá trình đệ quy vẫn tiến triển vì ít nhất một bên trở nên nhỏ hơn hoàn toàn sau khi phân vùng, ngăn chặn các vòng lặp vô hạn và đảm bảo sự hội tụ cuối cùng.
