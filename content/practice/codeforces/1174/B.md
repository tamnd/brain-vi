---
title: "CF 1174B - Ehab là một người kỳ quặc"
description: "Chúng ta được cung cấp một dãy số và chúng ta được phép sắp xếp lại nó, nhưng có một hạn chế về cách thức hoạt động của các giao dịch hoán đổi. Việc hoán đổi chỉ hợp pháp nếu chúng ta chọn hai vị trí có giá trị chẵn lẻ đối lập nhau, nghĩa là một vị trí là số lẻ và vị trí kia là số chẵn."
date: "2026-06-12T01:51:31+07:00"
tags: ["codeforces", "competitive-programming", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1174
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 563 (Div. 2)"
rating: 1200
weight: 1174
solve_time_s: 93
verified: true
draft: false
---

[CF 1174B - Ehab là một người kỳ quặc](https://codeforces.com/problemset/problem/1174/B) 

**Đánh giá:** 1200 
**Thẻ:** sắp xếp 
**Thời gian giải:** 1m 33s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một dãy số và chúng ta được phép sắp xếp lại nó, nhưng có một hạn chế về cách thức hoạt động của các giao dịch hoán đổi. Việc hoán đổi chỉ hợp pháp nếu chúng ta chọn hai vị trí có giá trị chẵn lẻ đối lập nhau, nghĩa là một vị trí là số lẻ và vị trí kia là số chẵn. Nếu cả hai đều chẵn hoặc cả hai đều lẻ thì chúng ta không thể hoán đổi chúng một cách trực tiếp. 

Nhiệm vụ là xác định mảng nhỏ nhất về mặt từ điển có thể truy cập được bằng cách sử dụng bất kỳ số lượng hoán đổi được phép nào như vậy. Thứ tự từ điển có nghĩa là chúng ta so sánh các mảng từ trái sang phải và ưu tiên vị trí đầu tiên nơi chúng khác nhau. 

Ý nghĩa chính của hoạt động này là các nhóm chẵn lẻ hoạt động khác nhau: các phần tử chỉ có thể di chuyển tự do qua các ranh giới chẵn lẻ, không nằm trong cùng một nhóm chẵn lẻ. Điều này ngay lập tức cho thấy rằng không thể hoán vị hoàn toàn tùy ý mà chỉ có thể sắp xếp lại một cách hạn chế. 

Ràng buộc của n lên tới 100.000 ngụ ý rằng chúng ta cần ít nhất giải pháp O(n log n) hoặc O(n). Bất kỳ cách tiếp cận nào liên quan đến việc mô phỏng lặp đi lặp lại các giao dịch hoán đổi hoặc khám phá cấu hình theo kiểu BFS đều không khả thi vì không gian trạng thái là hàm mũ theo n. 

Một trường hợp phức tạp nhưng quan trọng phát sinh khi tất cả các phần tử có cùng tính chẵn lẻ. Nếu tất cả các số đều là số chẵn hoặc tất cả đều là số lẻ thì không có hoán đổi hợp lệ nào tồn tại vì mọi cặp đều có tổng là một số chẵn. Trong trường hợp này, mảng đã được cố định và không thể thay đổi được. 

Một trường hợp đặc biệt khác là khi có sự kết hợp của các số chẵn lẻ nhưng một nỗ lực tham lam ngây thơ cố gắng sắp xếp toàn bộ mảng. Ví dụ, đầu vào`[3, 2, 1]`có thể cám dỗ sắp xếp để`[1, 2, 3]`, nhưng tính khả thi phụ thuộc vào tương tác chẵn lẻ chứ không phải thứ tự thuần túy. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô phỏng tất cả các giao dịch hoán đổi hợp lệ và cố gắng đạt được tất cả các hoán vị có thể tiếp cận, sau đó chọn từ điển nhỏ nhất. Từ góc độ trạng thái, mỗi cấu hình có thể có tối đa O(n2) giao dịch hoán đổi và có n! tổng số hoán vị, làm cho điều này hoàn toàn không khả thi. 

Quan sát quan trọng là việc hoán đổi được cho phép chính xác khi hai phần tử có tính chẵn lẻ khác nhau. Điều này có nghĩa là bất kỳ phần tử lẻ nào cũng có thể hoán đổi với bất kỳ phần tử chẵn nào và ngược lại. Qua các lần hoán đổi lặp đi lặp lại, điều này ngụ ý rằng tất cả các tỷ lệ cược có thể hoán vị một cách hiệu quả với tất cả các số chẵn thông qua các trung gian, nhưng các yếu tố của cùng một mức chẵn lẻ không bao giờ trực tiếp hạn chế thứ tự của nhau ngoại trừ thông qua sự trộn lẫn toàn cầu. 

Chúng ta có thể diễn giải lại quy trình: chúng ta được phép sắp xếp lại thứ tự nhiều tập hợp của tất cả các số một cách tùy ý, ngoại trừ các tương tác chẵn lẻ đảm bảo rằng thứ tự tương đối của các nhóm chẵn lẻ trong sắp xếp cuối cùng chỉ bị hạn chế bởi cách các phần tử được xen kẽ. 

Sự đơn giản hóa chính là chúng ta có thể coi tất cả các phần tử thuộc về hai nhóm: tỷ lệ cược và số chẵn. Trong mỗi nhóm, thứ tự tương đối là hoàn toàn miễn phí vì chúng ta có thể sử dụng các phần tử có tính chẵn lẻ đối lập làm trung gian để mô phỏng hiệu quả các giao dịch hoán đổi giữa các phần tử có cùng tính chẵn lẻ. Đây là một lập luận về tính bắc cầu tiêu chuẩn: các hoán đổi chẵn-lẻ cho phép chúng ta "bong bóng" các phần tử thông qua nhóm khác. 

Do đó, cả hai nhóm chẵn và lẻ đều có thể được sắp xếp độc lập và sau đó được hợp nhất một cách tham lam bằng cách đặt phần tử nhỏ nhất có sẵn ở mỗi vị trí, vì chúng ta luôn có thể chọn từ một trong hai nhóm mà không vi phạm các ràng buộc. 

Tuy nhiên, ràng buộc thực sự đơn giản hơn: bởi vì bất kỳ số lẻ nào cũng có thể hoán đổi với bất kỳ số chẵn nào, nên đa tập hợp hoàn toàn có thể hoán vị. Điều này có nghĩa hạn chế duy nhất là chúng ta không thể phân biệt các lớp chẵn lẻ về mặt cấu trúc, do đó cách sắp xếp tối ưu chỉ đơn giản là mảng được sắp xếp toàn cục. 

Một quan niệm sai lầm ngây thơ là cho rằng các nhóm chẵn lẻ bị khóa bên trong, nhưng trên thực tế, biểu đồ hoán đổi được kết nối miễn là cả hai nhóm chẵn lẻ đều tồn tại. 

Vì vậy, logic đúng sẽ chia thành các trường hợp: 

Nếu cả số lẻ và số chẵn đều tồn tại, chúng ta có thể đạt được bất kỳ hoán vị nào, vì vậy chúng ta sắp xếp toàn bộ mảng. 

Nếu chỉ tồn tại một chẵn lẻ thì không thể hoán đổi được, vì vậy chúng tôi xuất ra mảng ban đầu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Hoán đổi vũ lực | Ồ (n!) | Ồ (n!) | Quá chậm | 
| Mô phỏng chẵn lẻ | O(n²) | O(n) | Quá chậm | 
| Sắp xếp bằng kiểm tra tính chẵn lẻ | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Quét mảng và đếm xem có bao nhiêu phần tử lẻ và bao nhiêu phần tử chẵn. Điều này xác định liệu có thể thực hiện bất kỳ giao dịch hoán đổi nào hay không, vì các giao dịch hoán đổi yêu cầu các giá trị chẵn lẻ đối diện. 
2. Nếu số lẻ hoặc số chẵn bằng 0, hãy trả về mảng không thay đổi. Không có trao đổi hợp lệ tồn tại, do đó không thể sắp xếp lại. 
3. Ngược lại, sắp xếp toàn bộ mảng theo thứ tự không giảm. Lý do là biểu đồ hoán đổi được kết nối khi cả hai số chẵn lẻ tồn tại, nghĩa là bất kỳ hoán vị nào cũng có thể được xây dựng. 
4. Xuất ra mảng đã sắp xếp dưới dạng sắp xếp nhỏ nhất theo từ điển. 

### Tại sao nó hoạt động 

Hoạt động được phép xác định biểu đồ trên các vị trí mảng trong đó các cạnh kết nối các phần tử có tính chẵn lẻ khác nhau. Khi có cả hai giá trị chẵn lẻ, biểu đồ này được kết nối thông qua các hoán đổi chẵn lẻ xen kẽ, cho phép bất kỳ phần tử nào được di chuyển đến bất kỳ vị trí nào thông qua các hoán đổi trung gian. Điều này ngụ ý khả năng tiếp cận hoán vị đầy đủ. Vì tất cả các hoán vị đều có thể truy cập được nên cách sắp xếp có thể truy cập nhỏ nhất về mặt từ điển chỉ đơn giản là thứ tự được sắp xếp toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n = int(input())
    a = list(map(int, input().split()))
    
    odd = sum(x % 2 for x in a)
    even = n - odd
    
    if odd == 0 or even == 0:
        print(*a)
    else:
        a.sort()
        print(*a)

if __name__ == "__main__":
    main()
```Đầu tiên, mã sẽ đọc mảng và đếm phân phối chẵn lẻ. Việc kiểm tra đó rất quan trọng vì nó xác định liệu các giao dịch hoán đổi có thực sự thay đổi cấu hình hay không. Nếu không tồn tại cặp chẵn lẻ chéo, việc sắp xếp sẽ không hợp lệ vì nó giả định tính di động không tồn tại. 

Khi cả hai số chẵn lẻ tồn tại, chúng ta sắp xếp toàn bộ mảng. Điều này dựa trên thực tế là hoạt động hoán đổi có hiệu quả làm cho nhóm hoán vị có thể truy cập đầy đủ. 

Một điểm tinh tế là chúng ta không cần mô phỏng rõ ràng các giao dịch hoán đổi hoặc xây dựng các trạng thái trung gian. Đối số kết nối đảm bảo sự tồn tại của một chuỗi các hoán đổi hợp lệ chuyển đổi bất kỳ hoán vị nào thành bất kỳ hoán vị nào khác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
4 1 7
```Vì có cả số chẵn (4) và số lẻ (1, 7) nên việc sắp xếp được cho phép. 

| Bước | Trạng thái mảng | Số lẻ | Tính chẵn | 
| --- | --- | --- | --- | 
| Ban đầu | [4, 1, 7] | 2 | 1 | 
| Sắp xếp | [1, 4, 7] | 2 | 1 | 

Điều này xác nhận rằng với tính chẵn lẻ hỗn hợp, sự sắp xếp từ điển nhỏ nhất đạt được bằng cách sắp xếp. 

### Ví dụ 2 

đầu vào:```
4
2 6 8 10
```Tất cả các số đều là số chẵn nên không được phép hoán đổi. 

| Bước | Trạng thái mảng | Hành động | 
| --- | --- | --- | 
| Ban đầu | [2, 6, 8, 10] | Không thể hoán đổi | 

Đầu ra không thay đổi. 

Điều này thể hiện hành vi ở trạng thái đóng băng khi điều kiện hoán đổi không bao giờ được thỏa mãn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế khi cả hai chẵn lẻ tồn tại | 
| Không gian | O(n) | Lưu trữ cho mảng | 

Các ràng buộc cho phép tối đa 100.000 phần tử, do đó, giải pháp O(n log n) nằm trong giới hạn. Kiểm tra tính chẵn lẻ là O(n) và không đáng kể so với việc sắp xếp. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    data = sys.stdin.read().strip().split()
    n = int(data[0])
    a = list(map(int, data[1:]))
    
    odd = sum(x % 2 for x in a)
    if odd == 0 or odd == n:
        return " ".join(map(str, a))
    a.sort()
    return " ".join(map(str, a))

# provided sample
assert run("3\n4 1 7\n") == "1 4 7", "sample 1"

# all even
assert run("4\n2 6 8 10\n") == "2 6 8 10", "all even"

# all odd
assert run("3\n5 1 7\n") == "5 1 7", "all odd"

# already sorted
assert run("5\n1 2 3 4 5\n") == "1 2 3 4 5", "already sorted"

# reverse order
assert run("5\n5 4 3 2 1\n") == "1 2 3 4 5", "mixed parity sortable"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả thậm chí | không thay đổi | không thể hoán đổi | 
| tất cả đều kỳ quặc | không thay đổi | không thể hoán đổi | 
| đã được sắp xếp | cùng một mảng | trường hợp nhận dạng | 
| thứ tự ngược lại | mảng được sắp xếp | sắp xếp lại đầy đủ | 

## Vỏ cạnh 

Khi tất cả các phần tử có cùng tính chẵn lẻ, thuật toán sẽ phát hiện chính xác rằng không thể áp dụng thao tác nào. Đối với đầu vào`[2, 6, 8]`, số lẻ bằng 0 nên ta trực tiếp trả về`[2, 6, 8]`. Mọi nỗ lực sắp xếp ở đây sẽ không hợp lệ vì không có sự hoán đổi pháp lý nào để biện minh cho việc thay đổi vị trí. 

Khi cả hai giá trị chẵn lẻ tồn tại, ngay cả khi các phần tử được xen kẽ nhiều, chẳng hạn như`[100, 1, 99, 2]`, thuật toán sắp xếp thành`[1, 2, 99, 100]`. Đối số kết nối đảm bảo rằng mọi chuyển vị bắt buộc có thể được phân tách thành các hoán đổi chẵn-lẻ hợp lệ, do đó kết quả được sắp xếp luôn có thể truy cập được.
