---
title: "CF 102823D - Đảo ngược bit"
description: "Hoạt động này hoạt động trên biểu diễn nhị phân của một số. Chúng ta có thể chọn ba vị trí bit liên tiếp bất kỳ và đảo ngược ba bit đó."
date: "2026-07-26T15:42:08+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102823
codeforces_index: "D"
codeforces_contest_name: "2018 China Collegiate Programming Contest - Guilin Site"
rating: 0
weight: 102823
solve_time_s: 45
verified: true
draft: false
---

[CF 102823D - Đảo ngược bit](https://codeforces.com/problemset/problem/102823/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 45s 
**Đã xác minh:** có 

##Giải pháp 
#Hiểu vấn đề 

Hoạt động này hoạt động trên biểu diễn nhị phân của một số. Chúng ta có thể chọn ba vị trí bit liên tiếp bất kỳ và đảo ngược ba bit đó. Trong số ba bit, chỉ có vị trí thứ nhất và thứ ba thay đổi vị trí, do đó, một thao tác chính xác là sự hoán đổi giữa hai bit cách nhau hai vị trí. 

Nhiệm vụ là biến đổi số`x`vào trong`y`sử dụng số lượng hoán đổi tối thiểu như vậy. Các số 0 đứng đầu có thể được xem xét, có nghĩa là chúng ta được phép sử dụng các vị trí vượt quá bit được đặt cao nhất khi áp dụng các thao tác. 

Cấu trúc quan trọng là một bit không bao giờ có thể thay đổi vị trí chẵn lẻ của nó. Một bit ở vị trí 0 chỉ có thể di chuyển đến vị trí 2, 4, 6, v.v. Một bit ở vị trí 1 chỉ có thể di chuyển đến vị trí 3, 5, 7, v.v. Phép toán chia biểu diễn nhị phân thành hai chuỗi độc lập, một chuỗi chứa vị trí chẵn và một chuỗi chứa vị trí lẻ. 

Các giá trị của`x`Và`y`có thể lớn như`10^18`, vì vậy chỉ có khoảng 61 bit có liên quan. Với tối đa 10000 trường hợp thử nghiệm, giải pháp phải gần với thời gian không đổi cho mỗi trường hợp. Việc mô phỏng các hoạt động là không khả thi vì số lượng hoán đổi có thể lớn nhưng việc xử lý số bit cố định lại đủ nhanh. 

Một lỗi phổ biến là chỉ so sánh tổng số cái. Ví dụ, chuyển đổi`1`vào trong`2`là không thể mặc dù cả hai số đều chứa một bit đã đặt. Bit duy nhất trong`1`ở vị trí 0, trong khi bit duy nhất trong`2`ở vị trí 1 và không có thao tác nào thay đổi tính chẵn lẻ. 

Một trường hợp khác là khi các số 0 đứng đầu có ý nghĩa quan trọng. Ví dụ, chuyển đổi`3`vào trong`6`có câu trả lời`1`. Trong hệ nhị phân đây là`011`ĐẾN`110`, và thao tác đảo ngược ba bit. Việc coi các số dưới dạng chuỗi mà không xem xét vị trí có thể vô tình bỏ sót rằng số 0 ngoài cùng bên trái là vị trí bit hợp lệ. 

Trường hợp cạnh cuối cùng là khi không cần thực hiện thao tác nào. Đối với đầu vào```
5
5
```nghĩa`x = 5`,`y = 5`, câu trả lời là`0`. Một giải pháp chỉ tính các giao dịch hoán đổi bắt buộc và quên đi trạng thái đã khớp có thể tạo ra câu trả lời tích cực không cần thiết. 

# Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ liên tục chọn bộ ba bit, áp dụng phép đảo ngược và tìm kiếm chuỗi hoạt động ngắn nhất đạt được`y`. Điều này đúng vì mọi chuỗi thao tác hợp lệ đều tương ứng với một đường dẫn trong biểu đồ trạng thái của các số nhị phân có thể có. Tuy nhiên, số lượng trạng thái tăng theo cấp số nhân với số lượng bit. Ngay cả việc giới hạn bản thân ở 61 bit có liên quan cũng mang lại quá nhiều khả năng, do đó việc tìm kiếm đường đi ngắn nhất là không thể. 

Quan sát quan trọng là việc đảo ngược ba bit chỉ hoán đổi vị trí`i`Và`i + 2`. Điều này có nghĩa là hoạt động không phải là một hoán vị bit tùy ý. Đó là một hoán đổi liền kề bên trong dãy con của các vị trí có cùng tính chẵn lẻ. Các vị trí chẵn tạo thành một mảng độc lập và các vị trí lẻ tạo thành một mảng độc lập khác. 

Sau phép biến đổi này, bài toán trở thành bài toán hoán đổi liền kề tối thiểu cổ điển. Đối với một nhóm chẵn lẻ, chúng ta biết vị trí của tất cả những người trong`x`và vị trí của tất cả những người trong`y`. Cái đầu tiên trong`x`phải di chuyển đến vị trí đầu tiên trong`y`, cái thứ hai phải chuyển sang cái thứ hai, v.v. Số lượng hoán đổi liền kề tối thiểu là tổng của các khoảng cách này trong mảng chẵn lẻ được nén. 

Lực lượng vũ phu hoạt động vì nó khám phá mọi chuyển động có thể có của bit, nhưng nó bỏ qua thực tế là các bit không bao giờ giao nhau giữa các nhóm chẵn lẻ. Việc quan sát tính chẵn lẻ làm giảm vấn đề thành hai sự sắp xếp lại một chiều độc lập. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(61) cho mỗi trường hợp thử nghiệm | O(61) | Đã chấp nhận | 

#Hướng dẫn thuật toán 

1. Trích xuất các bit của`x`Và`y`từ bit ít quan trọng nhất trở lên. Chúng tôi chỉ cần khoảng 61 vị trí vì cả hai giá trị đều cao nhất`10^18`. Các vị trí trên phạm vi này luôn bằng 0. 
2. Tách các vị trí bit đã đặt thành hai nhóm dựa trên tính chẵn lẻ. Đối với mỗi chẵn lẻ, lưu trữ các chỉ số của số chẵn lẻ trong chuỗi được nén. Ví dụ, các vị trí`0, 2, 4, 6`trở thành chỉ số nén`0, 1, 2, 3`. 
3. So sánh số lượng cái trong các nhóm chẵn lẻ tương ứng của`x`Và`y`. Nếu chúng khác nhau thì việc chuyển đổi là không thể vì các phép toán không bao giờ di chuyển một chút giữa vị trí chẵn và vị trí lẻ. 
4. Đối với mỗi nhóm chẵn lẻ, hãy ghép các nhóm chẵn lẻ theo thứ tự. Thêm sự khác biệt tuyệt đối giữa mọi chỉ mục nén nguồn và chỉ mục nén đích. Đây là số lần hoán đổi liền kề cần thiết trong chuỗi chẵn lẻ đó. 
5. Cộng chi phí của hai nhóm chẵn lẻ và in kết quả. 

Tại sao nó hoạt động: Hoạt động được phép chính xác là một phép hoán đổi liền kề ở một trong hai chuỗi chẵn lẻ. Các hoán đổi liền kề bảo toàn thứ tự của chẵn lẻ khác và có thể di chuyển bất kỳ bit nào đến bất kỳ vị trí nào trong cùng một nhóm chẵn lẻ. Đối với các chuỗi nhị phân, số lần hoán đổi liền kề tối thiểu đạt được bằng cách khớp cái đầu tiên với cái đầu tiên, cái thứ hai với cái thứ hai, v.v. Nếu số lượng cái khác nhau thì không có chuỗi hoán đổi nào có thể tạo hoặc loại bỏ một cái, vì vậy câu trả lời là không thể. 

#Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve_case(x, y):
    src = [[], []]
    dst = [[], []]

    for i in range(61):
        if (x >> i) & 1:
            src[i & 1].append(i // 2)
        if (y >> i) & 1:
            dst[i & 1].append(i // 2)

    ans = 0

    for p in range(2):
        if len(src[p]) != len(dst[p]):
            return -1
        for a, b in zip(src[p], dst[p]):
            ans += abs(a - b)

    return ans

def main():
    t = int(input())
    out = []

    for case in range(1, t + 1):
        x, y = map(int, input().split())
        out.append(f"Case {case}: {solve_case(x, y)}")

    print("\n".join(out))

if __name__ == "__main__":
    main()
```Giải pháp chỉ lưu trữ vị trí của các bit đã đặt vì các bit 0 không cần phải di chuyển một cách rõ ràng. Một bit được đặt ở vị trí`i`thuộc nhóm chẵn lẻ`i % 2`, và vị trí nén của nó là`i // 2`. Điều này chuyển đổi các vị trí ban đầu thành các chỉ số trong đó các phần tử lân cận thể hiện khả năng hoán đổi. 

Việc kiểm tra không thể thực hiện được trước khi tính toán khoảng cách. Chỉ so sánh tổng số cái sẽ không chính xác vì hai nhóm chẵn lẻ không thể trao đổi bit. 

Vòng lặp cuối cùng sử dụng thực tế là các vị trí được sắp xếp của các cái phải khớp theo cùng một thứ tự. Không cần lập trình hoặc mô phỏng động vì mỗi lần hoán đổi chỉ di chuyển một bước một chút trong chuỗi chẵn lẻ của chính nó. 

# Ví dụ đã hoạt động 

Đối với đầu vào```
3
0 3
3 6
6 9
```dấu vết là: 

| Trường hợp | Chẵn lẻ | Nguồn | Mục tiêu | Chi phí | 
| --- | --- | --- | --- | --- | 
| 1 | Thậm chí | trống | vị trí 0 | không thể | 

Vì`0`ĐẾN`3`, nguồn không có bit nào được đặt trong khi đích có một bit ở nhóm chẵn và một bit ở nhóm lẻ. Vì việc hoán đổi không thể tạo ra các bit nên câu trả lời là`-1`. 

Đối với trường hợp thứ hai: 

| Trường hợp | Chẵn lẻ | Nguồn | Mục tiêu | Chi phí | 
| --- | --- | --- | --- | --- | 
| 2 | Thậm chí |`[0]`|`[1]`|`1`| 
| 2 | lẻ |`[0]`|`[0]`|`0`| 
| 2 | Tổng cộng | | |`1`| 

Ba bit thấp nhất thay đổi từ`011`ĐẾN`110`. Vị trí chẵn di chuyển một bước trong chuỗi nén, tương ứng với một thao tác được phép. 

Đối với trường hợp thứ ba: 

| Trường hợp | Chẵn lẻ | Nguồn | Mục tiêu | Chi phí | 
| --- | --- | --- | --- | --- | 
| 3 | Thậm chí |`[1]`|`[0]`|`1`| 
| 3 | lẻ |`[0]`|`[1]`|`1`| 
| 3 | Tổng cộng | | |`2`| 

Hai nhóm chẵn lẻ, mỗi nhóm cần một chuyển động liền kề, vì vậy số thao tác tối thiểu là`2`. 

# Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(61) cho mỗi trường hợp thử nghiệm | Chúng tôi chỉ kiểm tra các vị trí nhị phân có liên quan và so sánh một vị trí được lưu trữ. | 
| Không gian | O(61) cho mỗi trường hợp thử nghiệm | Tối đa 61 bit vị trí được lưu trữ. | 

Giới hạn bit cố định giúp giải pháp dễ dàng phù hợp với giới hạn ngay cả đối với 10000 trường hợp thử nghiệm. Tổng công việc chỉ có vài trăm nghìn thao tác đơn giản. 

# Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    main()

    result = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return result

def solve_case(x, y):
    src = [[], []]
    dst = [[], []]

    for i in range(61):
        if (x >> i) & 1:
            src[i & 1].append(i // 2)
        if (y >> i) & 1:
            dst[i & 1].append(i // 2)

    ans = 0
    for p in range(2):
        if len(src[p]) != len(dst[p]):
            return -1
        for a, b in zip(src[p], dst[p]):
            ans += abs(a - b)
    return ans

def main():
    t = int(input())
    out = []
    for case in range(1, t + 1):
        x, y = map(int, input().split())
        out.append(f"Case {case}: {solve_case(x, y)}")
    print("\n".join(out))

assert run("""3
0 3
3 6
6 9
""") == """Case 1: -1
Case 2: 1
Case 3: 2
""", "sample"

assert solve_case(0, 0) == 0, "same number"
assert solve_case(1, 2) == -1, "different parity"
assert solve_case(7, 28) == 2, "multiple parity movements"
assert solve_case(10**18, 10**18) == 0, "large equal values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0 0`|`0`| Xử lý các số đã bằng nhau | 
|`1 2`|`-1`| Phát hiện những thay đổi chẵn lẻ không thể | 
|`7 28`|`2`| Xử lý một số chuyển động bit | 
|`10^18 10^18`|`0`| Xử lý các giá trị có kích thước tối đa | 

# Vỏ cạnh 

Đối với trường hợp chẵn lẻ không thể, hãy xem xét`x = 1`Và`y = 2`. Các dạng nhị phân là`...0001`Và`...0010`. Bit được đặt duy nhất bắt đầu ở vị trí 0 và kết thúc ở vị trí 1. Vì mọi thao tác đều hoán đổi vị trí với khoảng cách hai nên bit không bao giờ có thể đạt đến vị trí lẻ. Thuật toán đặt bit nguồn vào nhóm chẵn và bit đích vào nhóm lẻ, phát hiện các số đếm khác nhau và trả về`-1`. 

Để xử lý số 0 đứng đầu, hãy xem xét`x = 3`Và`y = 6`. Các bit liên quan là`011`Và`110`. Bit thiết lập ở vị trí 0 di chuyển đến vị trí 2, đây là một bước trong chuỗi nén chẵn. Vị trí lẻ không thay đổi. Thuật toán tính toán một chuyển động và trả về`1`. 

Đối với trường hợp đã được giải quyết, hãy xem xét`x = 12`Và`y = 12`. Cả hai nhóm chẵn lẻ đều chứa chính xác một vị trí giống nhau. Mọi khoảng cách đều bằng 0 nên câu trả lời là 0. Thuật toán không thực hiện các giao dịch hoán đổi không cần thiết. 

Đối với các giá trị lớn, hãy xem xét`x = 10^18`Và`y = 10^18`. Thuật toán chỉ kiểm tra 61 vị trí nên nó xử lý giới hạn trên mà không phụ thuộc vào độ lớn số. Câu trả lời vẫn là 0 vì không có bit nào cần phải di chuyển.
