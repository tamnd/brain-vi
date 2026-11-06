---
problem: 1265B
contest_id: 1265
problem_index: B
name: "Beautiful Numbers"
contest_name: "Codeforces Round 604 (Div. 2)"
rating: 1300
tags: ["data structures", "implementation", "math", "two pointers"]
answer: passed_samples
verified: true
solve_time_s: 171
date: 2026-06-13
model: gpt-5-3-mini
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a2d863c-a648-83ec-8f8b-4b13830ade05
---

#CF 1265B - Những con số đẹp 

**Đánh giá:** 1300 
**Tags:** cấu trúc dữ liệu, cách triển khai, toán học, hai con trỏ 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 2m 51s 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a2d863c-a648-83ec-8f8b-4b13830ade05 

--- 

##Giải pháp 

## Hiểu vấn đề 

Chúng ta được cho một hoán vị các số từ 1 đến n và chúng ta muốn phân loại từng giá trị m từ 1 đến n là “tốt” hoặc “xấu”. Giá trị m được coi là tốt nếu ở đâu đó bên trong mảng tồn tại một đoạn liền kề có các phần tử chính xác là các số từ 1 đến m, mỗi đoạn xuất hiện một lần. 

Vì vậy, thay vì chọn các giá trị tùy ý, chúng ta đang hỏi liệu hoán vị có chứa một khối các vị trí liên tiếp có giá trị tạo thành một tập hợp hoàn hảo gồm m số nguyên đầu tiên hay không. 

Đầu ra của mỗi test là một chuỗi nhị phân có độ dài n. Ký tự thứ i cho biết tôi có giỏi hay không. 

Các ràng buộc rất quan trọng vì n có thể lớn trong các trường hợp thử nghiệm, tổng cộng lên tới 2×10^5. Bất kỳ giải pháp nào thử tất cả các mảng con và kiểm tra xem chúng có hình thành các hoán vị hợp lệ hay không sẽ liên tục quét các phân đoạn, dẫn đến hành vi bậc hai hoặc tệ hơn trên mỗi trường hợp thử nghiệm, vượt xa giới hạn 1 giây cho phép. 

Một cạm bẫy ngây thơ xuất hiện khi nghĩ “chỉ cần kiểm tra tất cả các mảng con và kiểm tra xem chúng có chứa chính xác 1..m” hay không. Ngay cả khi việc kiểm tra một mảng con được tối ưu hóa thì số lượng mảng con là O(n^2) và mỗi lần xác thực tốn ít nhất O(n) trừ khi được tối ưu hóa nhiều, điều này trở nên không khả thi. 

Một vấn đề tế nhị khác là giả định rằng nếu một phân đoạn chứa tất cả các giá trị từ 1 đến m ở đâu đó thì phân đoạn đó sẽ tự động hợp lệ. Điều đó không đúng trừ khi độ dài đoạn chính xác là m và không chứa số bổ sung nào ngoài phạm vi đó. 

## Phương pháp tiếp cận 

Phối cảnh brute-force bắt đầu bằng cách sửa m và quét tất cả các mảng con có thể. Đối với mỗi mảng con, chúng tôi sẽ theo dõi xem nó có chứa chính xác tập hợp {1, 2, ..., m} hay không. Điều này yêu cầu kiểm tra cả tính đầy đủ và sự vắng mặt của các giá trị bổ sung ngoài phạm vi. Ngay cả với mảng tần số, chúng ta vẫn cần các mảng con O(n^2) và cập nhật O(n) trong trường hợp xấu nhất, dẫn đến O(n^3) được triển khai đơn giản hoặc O(n^2) với quá trình xử lý trước nặng nề, vẫn không thành công đối với n lên đến 2×10^5. 

Quan sát quan trọng là chúng ta thực sự không cần tìm kiếm mảng con. Thay vào đó, chúng ta có thể đảo ngược phối cảnh: cố định m và xem vị trí của các số từ 1 đến m trong hoán vị. Các vị trí này xác định duy nhất bất kỳ phân khúc ứng viên nào có thể chứa tất cả các vị trí đó. 

Nếu một phân đoạn hợp lệ tồn tại, nó phải bao gồm tất cả các lần xuất hiện của các số từ 1 đến m. Đoạn nhỏ nhất như vậy là từ vị trí tối thiểu trong số các giá trị này đến vị trí tối đa trong số các giá trị này. Đoạn đó hợp lệ chính xác khi nó không chứa phần tử bổ sung nào ngoài m số đó, điều này xảy ra chính xác khi độ dài của nó bằng m. 

Điều này chuyển vấn đề thành việc theo dõi các vị trí tối thiểu và tối đa một cách linh hoạt khi m tăng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^3) hoặc O(n^2 log n) | O(n) | Quá chậm | 
| Tối ưu (theo dõi vị trí) | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi tính toán vị trí của mọi giá trị trong hoán vị để có thể nhanh chóng xác định vị trí mỗi số xuất hiện.

1. Xây dựng một mảng pos trong đó pos[x] là chỉ số của giá trị x trong hoán vị. Điều này cho phép tra cứu liên tục vị trí của mỗi số. 
2. Khởi tạo hai biến min_pos và max_pos với vị trí giá trị 1. Tại thời điểm này, chỉ số 1 được xem xét và phân đoạn chứa nó có giá trị tầm thường. 
3. Lặp lại m từ 2 đến n. Với mỗi m, hãy cập nhật min_pos và max_pos bằng pos[m]. Chúng đại diện cho các chỉ số nhỏ nhất và lớn nhất trong số các giá trị từ 1 đến m. 
4. Sau khi cập nhật cho mỗi m, hãy kiểm tra xem max_pos - min_pos + 1 có bằng m hay không. Điều kiện này có nghĩa là khoảng bao gồm tất cả các lần xuất hiện của 1..m có chính xác m phần tử, do đó không có số thừa nào có thể nằm bên trong nó. 
5. Nếu điều kiện đúng, đánh dấu m là đẹp; nếu không, hãy đánh dấu nó là không đẹp. 

### Tại sao nó hoạt động 

Tại bất kỳ bước m nào, tập hợp các vị trí của các số từ 1 đến m là cố định. Bất kỳ phân đoạn hợp lệ nào chứa tất cả các giá trị này phải bao gồm ít nhất từ ​​min_pos đến max_pos. Nếu khoảng đó có chính xác m phần tử thì nó chứa chính xác m vị trí đó và không có gì khác. Vì mảng là một hoán vị nên các vị trí đó tương ứng với các giá trị riêng biệt nên đoạn này phải là hoán vị 1..m. Nếu khoảng lớn hơn, nó sẽ bao gồm các phần tử bổ sung bên ngoài tập hợp, khiến cho không thể hình thành một hoán vị rõ ràng có kích thước m. 

Điều này có nghĩa là giá trị của m chỉ phụ thuộc vào hình học của các vị trí chứ không phụ thuộc vào việc kiểm tra mảng con rõ ràng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        p = list(map(int, input().split()))

        pos = [0] * (n + 1)
        for i, v in enumerate(p):
            pos[v] = i

        min_pos = pos[1]
        max_pos = pos[1]

        ans = ["0"] * (n + 1)

        for m in range(1, n + 1):
            idx = pos[m]
            if m == 1:
                min_pos = max_pos = idx
            else:
                if idx < min_pos:
                    min_pos = idx
                if idx > max_pos:
                    max_pos = idx

            if max_pos - min_pos + 1 == m:
                ans[m] = "1"

        out.append("".join(ans[1:]))

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Giải pháp dựa hoàn toàn vào việc duy trì phạm vi vị trí của các giá trị từ 1 đến m. Mảng pos đảm bảo quyền truy cập liên tục vào vị trí của từng phần tử. Bản cập nhật gia tăng tránh tính toán lại mọi thứ từ đầu cho mỗi m. 

Một lỗi triển khai phổ biến là tính toán lại các vị trí tối thiểu và tối đa bằng cách quét tất cả các giá trị trước đó cho mỗi m, điều này sẽ làm giảm hiệu suất xuống O(n^2). Một vấn đề tế nhị khác là quên rằng việc lập chỉ mục dựa trên số 0 trong Python, do đó việc tính toán độ dài khoảng thời gian phải nhất quán với lựa chọn đó. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hoán vị đầu vào: [4, 5, 1, 3, 2, 6] 

Chúng tôi tính toán các vị trí: 

| m | vị trí[m] | phút_pos | max_pos | tối đa-tối thiểu+1 | đẹp | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 2 | 2 | 1 | 1 | 
| 2 | 4 | 2 | 4 | 3 | 0 | 
| 3 | 3 | 2 | 4 | 3 | 1 | 
| 4 | 0 | 0 | 4 | 5 | 0 | 
| 5 | 1 | 0 | 4 | 5 | 1 | 
| 6 | 5 | 0 | 5 | 6 | 1 | 

Bảng cho thấy các giá trị hợp lệ xuất hiện chính xác khi các vị trí 1..m tạo thành một khoảng chặt chẽ không có khoảng trống. 

### Ví dụ 2 

Hoán vị đầu vào: [1, 4, 3, 2] 

| m | vị trí[m] | phút_pos | max_pos | tối đa-tối thiểu+1 | đẹp | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 0 | 0 | 1 | 1 | 
| 2 | 3 | 0 | 3 | 4 | 0 | 
| 3 | 2 | 0 | 3 | 4 | 0 | 
| 4 | 1 | 0 | 3 | 4 | 1 | 

Điều này chứng tỏ rằng ngay cả khi các giá trị không theo thứ tự, cấu trúc chỉ phụ thuộc vào khoảng vị trí của chúng chứ không phụ thuộc vào sự sắp xếp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi giá trị được xử lý một lần và cập nhật vị trí theo thời gian không đổi | 
| Không gian | O(n) | Lưu trữ mảng vị trí và chuỗi đầu ra | 

Tổng độ phức tạp trong tất cả các trường hợp thử nghiệm vẫn tuyến tính ở kích thước đầu vào, dễ dàng phù hợp với giới hạn 2×10^5 phần tử. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    res = []
    for _ in range(t):
        n = int(input())
        p = list(map(int, input().split()))
        pos = [0]*(n+1)
        for i,v in enumerate(p):
            pos[v]=i
        mn=pos[1]; mx=pos[1]
        ans=["0"]*(n+1)
        for m in range(1,n+1):
            idx=pos[m]
            if idx<mn: mn=idx
            if idx>mx: mx=idx
            if mx-mn+1==m:
                ans[m]="1"
        res.append("".join(ans[1:]))
    return "\n".join(res)

# provided samples
assert run("3\n6\n4 5 1 3 2 6\n5\n5 3 1 2 4\n4\n1 4 3 2\n") == "101011\n11111\n1001"

# minimum size
assert run("1\n1\n1\n") == "1"

# already sorted
assert run("1\n5\n1 2 3 4 5\n") == "11111"

# reverse
assert run("1\n5\n5 4 3 2 1\n") == "11111"

# random structure
assert run("1\n6\n2 1 4 3 6 5\n") == "110101"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cỡ 1 | 1 | tính đúng đắn của trường hợp cơ sở | 
| hoán vị được sắp xếp | 11111 | cấu trúc đơn điệu | 
| hoán vị ngược | 11111 | đặt hàng không liên quan | 
| hoán đổi theo cặp | 110101 | tính đúng đắn chung | 

## Vỏ cạnh 

Trường hợp một cạnh là hoán vị nhỏ nhất có thể. Với một phần tử duy nhất, m hợp lệ duy nhất là 1 và thuật toán khởi tạo chính xác min_pos và max_pos tại chỉ mục đơn đó, tạo ra một khoảng có độ dài 1. 

Một trường hợp khác là hoán vị đảo ngược hoàn toàn. Mặc dù các phần tử còn lâu mới được sắp xếp, nhưng mỗi tiền tố 1..m vẫn chiếm một phân đoạn liền kề trong không gian chỉ mục, do đó mỗi m đều vượt qua bài kiểm tra khoảng cách. Thuật toán nắm bắt được điều này vì phạm vi vị trí chỉ phụ thuộc vào điểm cuối chứ không phụ thuộc vào thứ tự. 

Một trường hợp tinh tế hơn là khi các giá trị được xen kẽ, chẳng hạn như xen kẽ các số lớn và nhỏ. Thuật toán không thực hiện đúng các giá trị m trung gian trong đó các vị trí 1..m không liền kề nhau trong không gian chỉ mục, ngay cả khi chúng xuất hiện thường xuyên ở gần nhau.
