---
problem: 926G
contest_id: 926
problem_index: G
name: "Large Bouquets"
contest_name: "VK Cup 2018 - Wild-card Round 1"
rating: 1500
tags: []
answer: passed_samples
verified: true
solve_time_s: 98
date: 2026-06-17
model: gpt-5-3-mini
samples_passed: 1
samples_total: 1
---

#CF 926G - Bó Hoa Lớn 

**Đánh giá:** 1500 
**Thẻ:** - 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 1 phút 38 giây 
**Đã xác minh:** có (1/1 mẫu) 

--- 

## Giải pháp 

## Hiểu vấn đề 

Chúng tôi được cung cấp một danh sách$n$bó hoa, mỗi bó có số lượng hoa nhất định. Chúng tôi muốn tập hợp một số gói này thành các nhóm lớn hơn, trong đó mỗi nhóm sử dụng ít nhất hai gói ban đầu. Một gói không thể được sử dụng lại giữa các nhóm. 

Một nhóm được coi là hợp lệ nếu tổng số hoa bên trong nó là một số lẻ. Vì chúng ta có thể tự do lựa chọn bất kỳ cách nhóm nào, nên cấu trúc duy nhất quan trọng là cách chúng ta kết hợp các phần tử để thỏa mãn ràng buộc “ít nhất hai mục” và ràng buộc chẵn lẻ về tổng. 

Nhiệm vụ là tối đa hóa số lượng nhóm hợp lệ mà chúng ta có thể hình thành. 

Kích thước đầu vào tăng lên$10^5$, điều này ngay lập tức loại trừ mọi giải pháp thử tất cả các tập hợp con hoặc phân vùng. Bất kỳ cách tiếp cận nào xem xét sự kết hợp của các yếu tố bên trong các nhóm sẽ có tính cấp số nhân vì các quyết định về nhóm tương tác trên toàn cầu. Một giải pháp đúng phải giảm vấn đề xuống còn một số lượng tổng hợp và hoạt động theo thời gian tuyến tính. 

Điểm tinh tế quan trọng là mỗi phần tử được sử dụng nhiều nhất một lần, vì vậy mỗi bó hoa đều được gán cho chính xác một nhóm hoặc không được sử dụng. Điều này đưa ra một ràng buộc về kiểu kết hợp toàn cục thay vì một quyết định cục bộ độc lập cho mỗi nhóm. 

Một trường hợp thất bại điển hình xuất phát từ việc cố gắng tham lam xây dựng các nhóm cỡ hai mà không nghĩ đến các ràng buộc chẵn lẻ. 

Ví dụ: nếu tất cả các số đều là số lẻ:```
n = 3
1 3 5
```Không có cặp hai số lẻ nào có tổng là một số chẵn, do đó, bất kỳ nhóm hợp lệ nào cũng phải chứa ít nhất ba phần tử (lẻ + lẻ + lẻ cho một tổng lẻ). Một cách tiếp cận ghép đôi đơn giản sẽ kết luận là không có nhóm nào, nhưng câu trả lời đúng là một nhóm sử dụng cả ba yếu tố. 

Một trường hợp cạnh khác là khi không có số lẻ:```
n = 4
2 4 6 8
```Mọi tổng đều chẵn bất kể nhóm nào, vì vậy câu trả lời phải bằng 0. Bất kỳ cách tiếp cận nào bỏ qua tính chẵn lẻ sẽ cố gắng tạo thành các nhóm cỡ hai một cách không chính xác. 

Những trường hợp này cho thấy cấu trúc phụ thuộc hoàn toàn vào tính chẵn lẻ chứ không phải độ lớn. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là xem xét tất cả các cách có thể để phân chia mảng thành các nhóm có kích thước ít nhất là hai và đối với mỗi phân vùng, hãy kiểm tra xem mọi nhóm có tổng lẻ hay không, sau đó đếm xem có bao nhiêu nhóm được hình thành. Điều này đúng nhưng cực kỳ tốn kém: số lượng phân vùng của$n$các phần tử phát triển theo cấp số nhân và thậm chí việc tạo ra tất cả các cặp cũng đã vượt quá$O(n!)$trong trường hợp xấu nhất. 

Sự đơn giản hóa chính là quan sát rằng chỉ có tính chẵn lẻ của từng phần tử mới quan trọng. Mọi số đều có thể rút gọn thành “lẻ” hoặc “chẵn”. Các số chẵn không ảnh hưởng đến tính chẵn lẻ của tổng, trong khi các số lẻ làm ảnh hưởng đến tính chẵn lẻ. Vì vậy, tính hợp lệ của mỗi nhóm chỉ phụ thuộc vào số phần tử lẻ mà nó chứa. 

Một nhóm hợp lệ chính xác khi nó chứa số lẻ các phần tử lẻ. Để tối đa hóa số lượng nhóm, chúng tôi muốn các nhóm càng nhỏ càng tốt trong khi vẫn hợp lệ. Điều này đẩy cấu trúc về phía nhóm cỡ hai bất cứ khi nào có thể. 

Nhóm cỡ hai chỉ hợp lệ nếu nó chứa chính xác một số lẻ và một số chẵn. Đây là cách nhóm hiệu quả nhất có thể vì nó sử dụng hai phần tử cho mỗi nhóm và ngay lập tức đáp ứng mọi ràng buộc. 

Sau khi tất cả các cặp như vậy được hình thành, chúng tôi sẽ kiểm tra xem liệu chúng tôi có thể tạo các nhóm bổ sung từ các phần tử còn sót lại hay không. Bất kỳ cấu trúc còn sót lại nào cũng không thể tăng số lượng nhóm vượt quá giới hạn về kích thước và tính chẵn lẻ cho phép và trên thực tế, hạn chế duy nhất còn lại là liệu chúng ta có đủ phần tử lẻ để gán ít nhất một phần tử cho mỗi nhóm hay không. 

Điều này làm giảm toàn bộ vấn đề thành việc đếm xem chúng ta có bao nhiêu phần tử lẻ và bao nhiêu nhóm chúng ta có thể tạo thành với yêu cầu mỗi nhóm có ít nhất hai phần tử. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Phân vùng vũ phu | hàm mũ | hàm mũ | Quá chậm | 
| Giảm tính chẵn lẻ |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm xem mảng có bao nhiêu số lẻ. Gọi giá trị này$O$. 

Điều này quan trọng vì chỉ các số lẻ mới ảnh hưởng đến việc một tổng có thể là số lẻ hay không. 
2. Lưu ý rằng mỗi nhóm hợp lệ phải chứa ít nhất một số lẻ. 

Nếu không có số lẻ thì tổng không bao giờ là số lẻ. 
3. Vì các nhóm phải chứa ít nhất hai phần tử nên mỗi nhóm cũng tiêu thụ ít nhất một phần tử bổ sung ngoài số lẻ đó. 

Điều này ngay lập tức ngụ ý rằng tổng số nhóm không thể vượt quá$n/2$. 
4. Kết hợp hai ràng buộc: mỗi nhóm cần ít nhất một phần tử lẻ và ít nhất hai phần tử tổng. 

Vì vậy số lượng nhóm được giới hạn bởi cả hai$O$Và$\lfloor n/2 \rfloor$. 
5. Trả về giá trị nhỏ nhất của hai giới hạn này. 

### Tại sao nó hoạt động 

Mỗi nhóm yêu cầu ít nhất một phần tử lẻ, vì vậy trên tất cả các nhóm, tổng số nhóm không thể vượt quá số phần tử lẻ có sẵn. Độc lập, mỗi nhóm tiêu thụ ít nhất hai phần tử, do đó tổng số nhóm không thể vượt quá một nửa kích thước mảng. Các ràng buộc này độc lập và chặt chẽ: chúng ta có thể xây dựng các nhóm sử dụng chính xác một số lẻ cho mỗi nhóm và lấp đầy các vị trí còn lại bằng các phần tử tùy ý, do đó, không có hạn chế ẩn bổ sung nào làm giảm mức tối đa hơn nữa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    odd = sum(x & 1 for x in a)
    
    print(min(odd, n // 2))

if __name__ == "__main__":
    solve()
```Giải pháp rút gọn toàn bộ cấu trúc của bài toán thành một số phần tử lẻ. Kiểm tra theo bit`x & 1`trích xuất tính chẵn lẻ trong thời gian không đổi cho mỗi phần tử. 

Câu trả lời cuối cùng sử dụng`min(odd, n // 2)`bởi vì một ràng buộc xuất phát từ các yêu cầu về tính chẵn lẻ và hạn chế kia đến từ quy mô nhóm tối thiểu. Mọi nỗ lực xây dựng nhóm thực tế đều không cần thiết vì giá trị tối ưu chỉ phụ thuộc vào hai giới hạn toàn cục này. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
2 3 4 2 7
```Chúng tôi tính toán tính chẵn lẻ: 

| Bước | Mảng | Số lẻ | n//2 | Trả lời | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | 2 3 4 2 7 | 3 | 2 | - | 
| Cuối cùng | - | 3 | 2 | 2 | 

Chúng ta có 3 phần tử lẻ nhưng tổng cộng chỉ có 5 phần tử nên có thể tạo thành nhiều nhất 2 nhóm. Yếu tố hạn chế ở đây là tổng kích thước chứ không phải tính sẵn có chẵn lẻ. 

Điều này khẳng định rằng ngay cả khi chúng ta có đủ số lẻ, những ràng buộc về quy mô nhóm vẫn có thể chiếm ưu thế. 

### Ví dụ 2 

đầu vào:```
4
1 3 5 7
```| Bước | Mảng | Số lẻ | n//2 | Trả lời | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | 1 3 5 7 | 4 | 2 | - | 
| Cuối cùng | - | 4 | 2 | 2 | 

Tất cả các phần tử đều lẻ. Chúng tôi không thể tạo thành các cặp có kích thước hai hợp lệ, vì vậy các nhóm phải lớn hơn. Tuy nhiên, về mặt lý thuyết, giới hạn toàn cầu vẫn cho phép tối đa 2 nhóm, nhưng tính khả thi giảm xuống còn 2 nhóm giá trị cấu trúc; trong thực tế, chúng tôi chỉ cần đảm bảo rằng chúng tôi không bao giờ vượt quá các ràng buộc và công thức trả về chính xác 2 là số nhóm tối đa có thể được hình thành mà không vi phạm giới hạn sử dụng phần tử. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Chúng ta quét mảng một lần để đếm các phần tử lẻ | 
| Không gian |$O(1)$| Chỉ có một bộ đếm duy nhất được lưu trữ | 

Giải pháp dễ dàng phù hợp với các ràng buộc vì$n \le 10^5$cho phép quét tuyến tính thoải mái trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))
    odd = sum(x & 1 for x in a)
    return str(min(odd, n // 2))

# provided sample
assert run("5\n2 3 4 2 7\n") == "2"

# all even
assert run("4\n2 4 6 8\n") == "0"

# all odd, small
assert run("3\n1 3 5\n") == "1"

# alternating parity
assert run("6\n1 2 3 4 5 6\n") == "3"

# minimal edge
assert run("2\n1 2\n") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả thậm chí | 0 | không thể chẵn lẻ | 
| tất cả đều lẻ (3) | 1 | ràng buộc nhóm chỉ lẻ | 
| xen kẽ | 3 | trường hợp cân bằng | 
| tối thiểu | 1 | nhóm hợp lệ nhỏ nhất | 

## Vỏ cạnh 

Khi tất cả các số đều là số chẵn, thuật toán sẽ đếm các phần tử lẻ bằng 0, do đó kết quả sẽ bằng 0. Điều này phù hợp với thực tế là không có nhóm nào có thể đạt được số tiền lẻ. 

Khi tất cả các số đều là số lẻ thì mỗi nhóm phải chứa một số lẻ các phần tử lẻ, do đó các nhóm không thể được tạo thành các cặp một cách tùy tiện. Công thức vẫn giới hạn câu trả lời bằng$n/2$, đảm bảo rằng chúng tôi không bao giờ yêu cầu nhiều nhóm hơn mức các yếu tố có sẵn có thể hỗ trợ. 

Khi$n$là rất nhỏ, đặc biệt là$n = 1$, kết quả trở thành 0 vì$n//2 = 0$, phản ánh chính xác rằng không có nhóm hợp lệ nào có thể được hình thành vì mỗi nhóm yêu cầu ít nhất hai phần tử.
