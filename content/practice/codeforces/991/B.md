---
problem: 991B
contest_id: 991
problem_index: B
name: "Getting an A"
contest_name: "Codeforces Round 491 (Div. 2)"
rating: 900
tags: ["greedy", "sortings"]
answer: passed_samples
verified: true
solve_time_s: 57
date: 2026-06-18
model: gpt-5-3-mini
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a33abdb-0708-83ec-9dd2-80237283c7dd
---

#CF 991B - Đạt điểm A 

**Xếp hạng:** 900 
**Tags:** tham lam, phân loại 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 57s 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a33abdb-0708-83ec-9dd2-80237283c7dd 

--- 

## Giải pháp 

## Hiểu vấn đề 

Chúng ta được cung cấp một danh sách các điểm trong phòng thí nghiệm, mỗi điểm từ 2 đến 5. Điểm khóa học được tính là trung bình số học của tất cả các điểm trong phòng thí nghiệm, sau đó được làm tròn đến số nguyên gần nhất. Quy tắc làm tròn là "làm tròn một nửa" tiêu chuẩn, nghĩa là các giá trị như 4,5 trở thành 5 trong khi 4,49 trở thành 4. 

Vasya có thể cải thiện điểm cuối cùng của mình bằng cách chọn một số công việc trong phòng thí nghiệm và làm lại chúng. Việc làm lại buộc lớp phòng thí nghiệm đó trở thành 5. Mỗi lần làm lại sẽ tăng tổng số điểm trong khi vẫn giữ nguyên số lượng phòng thí nghiệm, điều này làm tăng điểm trung bình. 

Mục tiêu là xác định số lượng công việc trong phòng thí nghiệm nhỏ nhất mà Vasya phải làm lại sao cho giá trị trung bình làm tròn trở thành 5. Vì ngưỡng làm tròn để đạt điểm 5 cuối cùng đạt mức trung bình ít nhất là 4,5 nên vấn đề giảm xuống để đảm bảo giá trị trung bình ít nhất là 4,5 sau khi nâng cấp. 

Các ràng buộc rất nhỏ, tối đa là 100 điểm. Điều này cho phép các giải pháp có tính chất bậc hai hoặc thậm chí là bậc ba, nhưng cũng làm cho các chiến lược sắp xếp tham lam trở thành ứng cử viên rất tự nhiên. 

Một trường hợp phức tạp xuất hiện khi mức trung bình đã gần bằng 4,5. Ví dụ: nếu tất cả các điểm là 4 ngoại trừ một điểm 5 thì điểm trung bình có thể đã vượt quá 4,5, nghĩa là không cần thực hiện thao tác nào. Một cách tiếp cận ngây thơ luôn cố gắng sửa đổi điểm sẽ làm tăng câu trả lời một cách không chính xác. 

Một trường hợp cạnh khác phát sinh từ hành vi làm tròn. Vì quy tắc là "làm tròn một nửa", điều kiện không hẳn lớn hơn 4,5 mà lớn hơn hoặc bằng 4,5. Việc thiếu ngưỡng này sẽ dẫn đến từng lỗi nhỏ trong việc quyết định xem cấu hình hiện tại đã đủ hay chưa. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là thử tăng các tập hợp con của điểm. Đối với mỗi số lần làm lại k có thể, chúng ta có thể chọn k vị trí bất kỳ, đặt chúng thành 5, tính giá trị trung bình mới và kiểm tra xem nó có đạt đến ngưỡng yêu cầu hay không. Điều này đúng vì nó khám phá một cách thấu đáo tất cả các kết hợp cải tiến. Tuy nhiên, tốc độ này quá chậm vì số lượng tập hợp con tăng lên một cách tổ hợp khi n tăng, đại khái theo thứ tự C(n, k) cho mỗi k, dẫn đến hành vi theo cấp số nhân. 

Quan sát quan trọng là việc cải thiện điểm thấp mang lại nhiều lợi ích hơn so với việc cải thiện điểm cao hơn. Thay thế 2 bằng 5 sẽ tăng tổng lên 3, trong khi thay 4 bằng 5 sẽ chỉ tăng tổng lên 1. Vì muốn đạt đến ngưỡng với các thao tác tối thiểu nên chúng ta phải luôn ưu tiên nâng cấp các giá trị nhỏ nhất trước tiên. Điều này biến vấn đề thành vấn đề lựa chọn tham lam sau khi sắp xếp. 

Sau khi các điểm được sắp xếp theo thứ tự tăng dần, chúng tôi mô phỏng chuyển đổi từng giá trị nhỏ nhất thành 5 giây. Sau mỗi lần chuyển đổi, chúng tôi cập nhật tổng và kiểm tra xem điều kiện trung bình có đáp ứng hay không. Thời điểm đầu tiên điều kiện được thỏa mãn sẽ đưa ra số lượng thao tác tối thiểu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n · n) | O(n) | Quá chậm | 
| Tham lam tối ưu + Sắp xếp | O(n log n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta chuyển bài toán sang kiểm tra xem có bao nhiêu phần tử nhỏ nhất phải được thay thế bằng 5 để đạt được giá trị trung bình ít nhất là 4,5. 

1. Tính tổng điểm của tất cả các lớp. Điều này đưa ra mức trung bình cơ bản hiện tại. 
2. Sắp xếp điểm theo thứ tự tăng dần sao cho điểm yếu nhất được xét trước để cải thiện. Điều này là tối ưu vì mỗi lần nâng cấp sẽ tăng thêm mức tăng tối đa có thể khi áp dụng cho các giá trị nhỏ nhất. 
3. Lặp lại xem chúng tôi quyết định nâng cấp bao nhiêu phần tử, từ 0 đến n. 
4. Với mỗi k, tính tổng mới bằng cách cộng (5 - a[i]) cho k phần tử nhỏ nhất. 
5. Kiểm tra xem điều kiện new_sum ≥ 4,5 * n có đúng không. 
6. K nhỏ nhất thỏa mãn điều kiện là đáp số.

Điểm quyết định quan trọng là bước 2. Nếu không sắp xếp, chúng ta có thể lãng phí hoạt động nâng cấp các cấp độ vốn đã cao, điều này mang lại ít sự cải thiện hơn cho mỗi hoạt động và có thể tăng số lần thực hiện lại cần thiết. 

### Tại sao nó hoạt động 

Quá trình này phụ thuộc vào một tính chất đơn điệu: thay thế một giá trị nhỏ hơn luôn mang lại mức tăng lớn hơn hoặc bằng tổng so với việc thay thế một giá trị lớn hơn. Điều này có nghĩa là bất kỳ chiến lược tối ưu nào cũng có thể được sắp xếp lại sao cho tất cả các lần làm lại đã chọn đều áp dụng cho các phần tử nhỏ nhất mà không làm xấu đi kết quả. Sau khi được sắp xếp, mọi tiền tố nâng cấp thể hiện sự cải thiện tốt nhất có thể đạt được với số lượng hoạt động đó, vì vậy tiền tố đầu tiên vượt qua ngưỡng phải tương ứng với mức tối ưu chung. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n = int(input())
    a = list(map(int, input().split()))
    
    total = sum(a)
    a.sort()
    
    need = 4.5 * n
    
    if total >= need:
        print(0)
        return
    
    for i in range(n):
        total += (5 - a[i])
        if total >= need:
            print(i + 1)
            return

if __name__ == "__main__":
    main()
```Lời giải bắt đầu bằng cách đọc số điểm và giá trị của chúng. Tổng số tiền được tính một lần, thể hiện trạng thái hiện tại trước khi có bất kỳ sửa đổi nào. 

Việc sắp xếp mảng là cần thiết vì nó đảm bảo rằng khi mô phỏng nâng cấp, chúng tôi luôn chọn các cấp nhỏ nhất trước tiên, tối đa hóa lợi ích trên mỗi thao tác. 

Sau đó chúng tôi tính ngưỡng là 4,5 lần n. Điều này tránh việc tính toán lại các giá trị trung bình nhiều lần và biến điều kiện thành một phép so sánh đơn giản về tổng. 

Vòng lặp dần dần áp dụng các cải tiến. Mỗi lần lặp lại mô phỏng việc làm lại thêm một phòng thí nghiệm bằng cách tăng tổng lên (5 - cấp hiện tại). Khi đạt đến ngưỡng, chúng tôi sẽ trả về ngay số lượng thao tác đã sử dụng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
4 4 4
```| k nâng cấp | giá trị đã chọn | tổng hợp sau khi nâng cấp | đáp ứng điều kiện | 
| --- | --- | --- | --- | 
| 0 | không | 12 | không | 
| 1 | 4 → 5 | 13 | không | 
| 2 | 4,4 → 5,5 | 14 | vâng | 

Ngưỡng trung bình là 13,5. Với một lần nâng cấp, số tiền vẫn còn quá thấp, nhưng với hai lần nâng cấp thì số tiền đã vượt quá ngưỡng. Điều này xác nhận rằng việc nhắm mục tiêu các giá trị nhỏ nhất là cần thiết vì tất cả các giá trị đều bằng nhau. 

### Ví dụ 2 

đầu vào:```
3
5 4 5
```| k nâng cấp | giá trị đã chọn | tổng hợp sau khi nâng cấp | đáp ứng điều kiện | 
| --- | --- | --- | --- | 
| 0 | không | 14 | vâng | 

Tổng ban đầu đã thỏa mãn ngưỡng 13,5 nên không cần thực hiện thao tác nào. Điều này chứng tỏ tầm quan trọng của việc kiểm tra điều kiện cơ bản trước khi áp dụng bất kỳ nâng cấp nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | sắp xếp chiếm ưu thế, quét tuyến tính sau đó | 
| Không gian | O(1) | chỉ có một vài biến ngoài lưu trữ đầu vào | 

Các ràng buộc giới hạn n tối đa là 100, do đó, ngay cả bước sắp xếp cũng có chi phí không đáng kể. Giải pháp dễ dàng phù hợp với giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))
    
    total = sum(a)
    a.sort()
    need = 4.5 * n
    
    if total >= need:
        return "0"
    
    for i in range(n):
        total += (5 - a[i])
        if total >= need:
            return str(i + 1)

    return str(n)

# provided samples
assert run("3\n4 4 4\n") == "2"

# custom cases
assert run("1\n5\n") == "0", "single max grade"
assert run("1\n2\n") == "1", "single minimum needs upgrade"
assert run("4\n5 5 5 5\n") == "0", "already perfect"
assert run("4\n2 2 2 2\n") == "2", "all low values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 5 | 0 | đã tối ưu | 
| 1 2 | 1 | trường hợp nâng cấp tối thiểu | 
| 5 5 5 5 | 0 | không cần thao tác | 
| 2 2 2 2 | 2 | tích lũy trong trường hợp xấu nhất | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi mức trung bình ban đầu đã vượt quá hoặc bằng 4,5. Ví dụ, đầu vào`3, 5 4 5`tạo ra tổng bằng 14 và ngưỡng là 13,5. Thuật toán xử lý việc này ngay lập tức trước bất kỳ nâng cấp nào, trả về 0 mà không cần vào vòng lặp. 

Một trường hợp khác là khi tất cả các giá trị đều giống nhau và ở mức thấp, chẳng hạn như`4, 4, 4`. Thuật toán xử lý các nâng cấp theo thứ tự và chỉ dừng lại khi tích lũy đủ mức tăng tích lũy. Mỗi bước sẽ liên tục cải tiến và vòng lặp đảm bảo thành công cuối cùng. 

Cuối cùng, kích thước đầu vào nhỏ nhất`n = 1`cư xử nhất quán. Nếu điểm duy nhất là 5 thì câu trả lời là 0. Nếu nó dưới 5, chỉ cần một lần nâng cấp là đủ vì giá trị sẽ trở thành 5 ngay lập tức, đảm bảo ngưỡng.
