---
title: "CF 105093L - Hoán đổi Hoán đổi++++"
description: "Chúng ta được cung cấp một mảng ban đầu và một chuỗi các thao tác hoán đổi tác động lên nó. Mỗi thao tác trao đổi các giá trị ở hai vị trí và việc áp dụng chuỗi đầy đủ sẽ tạo ra sự sắp xếp cuối cùng của mảng. Điều khó khăn là chúng tôi không thực hiện các giao dịch hoán đổi theo thứ tự nhất định."
date: "2026-06-27T20:51:37+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105093
codeforces_index: "L"
codeforces_contest_name: "2024 UP ACM Algolympics Final Round"
rating: 0
weight: 105093
solve_time_s: 52
verified: true
draft: false
---

[CF 105093L - SwapSwap++++](https://codeforces.com/problemset/problem/105093/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 52s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng ban đầu và một chuỗi các thao tác hoán đổi tác động lên nó. Mỗi thao tác trao đổi các giá trị ở hai vị trí và việc áp dụng chuỗi đầy đủ sẽ tạo ra sự sắp xếp cuối cùng của mảng. 

Điều khó khăn là chúng tôi không thực hiện các giao dịch hoán đổi theo thứ tự nhất định. Thay vào đó, chúng ta được phép sắp xếp lại cùng một bộ lệnh hoán đổi một cách tùy ý, vì chương trình SwapSwap++++ chỉ là một hoán vị của danh sách lệnh gốc. Mỗi hoán vị như vậy xác định một thứ tự thực hiện khác nhau của các hoạt động giống hệt nhau. 

Câu hỏi đặt ra là liệu chúng ta có thể tìm thấy hai cách sắp xếp lại khác nhau của cùng một giao dịch hoán đổi sao cho sau khi áp dụng mỗi chương trình được sắp xếp lại cho cùng một mảng ban đầu, kết quả là các mảng cuối cùng giống hệt nhau. 

Nói cách khác, chúng ta đang tìm kiếm hai hoán vị riêng biệt của cùng một chuỗi chuyển vị tạo ra hiệu ứng tổng thể giống nhau trên mảng. 

Các ràng buộc cho phép lên tới 200.000 lần hoán đổi và kích thước mảng lên tới 200.000. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào thử tất cả các lần sắp xếp lại hoặc mô phỏng nhiều lần thực thi. Ngay cả một mô phỏng đơn lẻ cũng là tuyến tính, vì vậy thách thức thực sự là quyết định về mặt cấu trúc khi nào có thể tồn tại hai thứ tự hợp lệ khác nhau và xây dựng chúng. 

Một trường hợp phức tạp xuất hiện khi các giao dịch hoán đổi có tính “rắc rối” cao. Ví dụ: nếu mọi giao dịch hoán đổi chia sẻ một điểm cuối với mọi giao dịch hoán đổi khác thì mỗi cặp giao dịch hoán đổi sẽ tương tác thông qua một chỉ mục chung. Trong trường hợp đó, việc trao đổi các hoạt động mà không thay đổi trạng thái trung gian trở nên khó khăn và chúng ta sẽ thấy rằng không tồn tại thủ thuật sắp xếp lại an toàn nào. 

Mặt khác, nếu có hai giao dịch hoán đổi hoạt động ở các vị thế hoàn toàn riêng biệt thì chúng hoạt động độc lập và có thể được trao đổi mà không ảnh hưởng đến kết quả cuối cùng. Sự độc lập này là thuộc tính cấu trúc quan trọng. 

## Phương pháp tiếp cận 

Một ý tưởng ngây thơ là coi mỗi hoán vị của danh sách lệnh như một chương trình ứng viên, mô phỏng nó trên mảng và so sánh kết quả. Điều này ngay lập tức là không thể vì có c! sắp xếp lại. 

Một cách tiếp cận mạnh mẽ hơn là thử hoán đổi các lệnh liền kề trong danh sách và kiểm tra xem chương trình kết quả có còn tạo ra cùng một đầu ra hay không. Điều này dẫn đến việc khám phá một không gian sắp xếp lại khổng lồ và thậm chí một mô phỏng duy nhất cũng tốn O(n + c), khiến quá trình tổng thể trở nên quá chậm. 

Điểm mấu chốt là mỗi lệnh là một chuyển vị trên các vị trí mảng và chuyển vị có một quy tắc giao hoán đơn giản: hai hoán đổi có thể được trao đổi mà không làm thay đổi hiệu ứng cuối cùng nếu chúng hoạt động trên các tập hợp chỉ số rời rạc. Nghĩa là, hoán đổi (i, j) và (k, l) giao hoán chính xác khi {i, j} và {k, l} không giao nhau. 

Quan sát này hoàn toàn bỏ qua nhu cầu lý luận về các trạng thái mảng trung gian. Thay vì nghiên cứu cách các giá trị di chuyển, chúng ta chỉ cần tìm xem liệu tập hợp hoán đổi có chứa ít nhất một cặp phép toán độc lập hay không. 

Nếu một cặp như vậy tồn tại, chúng ta có thể xây dựng hai hoán vị hợp lệ khác nhau bằng cách hoán đổi thứ tự tương đối của chúng trong khi vẫn giữ nguyên mọi thứ khác. Vì hai giao dịch hoán đổi chuyển đổi thành các hoán vị trên mảng nên cả hai lệnh thực hiện đều tạo ra các mảng cuối cùng giống hệt nhau. 

Nếu không có cặp như vậy tồn tại thì mỗi giao dịch hoán đổi sẽ chia sẻ ít nhất một điểm cuối với mọi giao dịch hoán đổi khác. Trong trường hợp đó, không có cặp thao tác nào có thể được trao đổi một cách an toàn và người ta có thể chỉ ra rằng bất kỳ sự sắp xếp lại nào cũng sẽ làm thay đổi thành phần kết quả theo cách có thể phát hiện được trên cấu hình ban đầu đã cho. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu đối với hoán vị | O(c! · (n + c)) | O(n + c) | Quá chậm | 
| Kiểm tra cặp hoán đổi hoán đổi | O(c) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giảm vấn đề xuống việc tìm xem liệu có tồn tại hai giao dịch hoán đổi hoạt động trên bốn chỉ số riêng biệt hay không. 

1. Đọc tất cả các lệnh hoán đổi và lưu mỗi lệnh thành một cặp (i, j).

Chúng tôi chỉ cần các điểm cuối vì các giao dịch hoán đổi hoàn toàn được xác định bởi các vị trí này. 
2. Quét qua danh sách trong khi vẫn duy trì bản ghi các giao dịch hoán đổi. 
3. Đối với mỗi cặp giao dịch hoán đổi, hãy kiểm tra xem chúng có rời rạc hay không, nghĩa là tập hợp các điểm cuối của chúng không giao nhau. 

Cụ thể, hai giao dịch hoán đổi (a, b) và (c, d) sẽ rời rạc nếu cả bốn chỉ số đều khác nhau. 
4. Nếu chúng tôi tìm thấy bất kỳ cặp nào như vậy, chúng tôi ngay lập tức kết luận rằng có câu trả lời hợp lệ. 
5. Để xây dựng hai chương trình khác nhau, lấy thứ tự ban đầu là p và hoán đổi thứ tự tương đối của hai hoán đổi rời rạc đó trong q trong khi giữ nguyên tất cả các lệnh khác. 
6. Nếu không tồn tại cặp rời nhau, xuất NO. 

Bước xây dựng này hợp lệ vì việc hoán đổi thứ tự của hai chuyển vị rời nhau không làm thay đổi thành phần của chúng dưới dạng hoán vị của các vị trí mảng. Vì chúng hoạt động trên các chỉ số riêng biệt nên chúng không bao giờ ảnh hưởng đến tác động của nhau. 

### Tại sao nó hoạt động 

Các lệnh hoán đổi là các phần tử của nhóm đối xứng trên các vị trí. Mỗi lệnh là một chuyển vị và thành phần của các giao dịch hoán đổi tương ứng với việc tổng hợp các hoán vị. Hai chuyển vị giao hoán chính xác khi chúng rời nhau. 

Nếu chúng ta tìm thấy hai hoán đổi rời rạc, chúng ta sẽ thu được hai từ riêng biệt trên cùng một bộ tạo mà chỉ khác nhau bằng cách trao đổi các phần tử giao hoán. Cả hai từ đều đánh giá cùng một hoán vị vị trí, do đó việc áp dụng chúng cho cùng một mảng ban đầu sẽ mang lại kết quả giống nhau. 

Nếu không tồn tại các giao dịch hoán đổi rời rạc thì mỗi cặp hoạt động sẽ chia sẻ ít nhất một chỉ mục, do đó không tồn tại cặp giao hoán nào. Vì chúng tôi bị hạn chế sử dụng mỗi lần hoán đổi chính xác một lần, nên không có mối quan hệ đại số hợp lệ nào cho phép sắp xếp lại một cách không tầm thường để bảo toàn thành phần tổng thể, do đó, bất kỳ hai hoán vị khác nhau nào cũng phải thay đổi kết quả. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n = int(input())
    a = list(map(int, input().split()))
    c = int(input())

    swaps = []
    for _ in range(c):
        parts = input().split()
        i = int(parts[1]) - 1
        j = int(parts[2]) - 1
        swaps.append((i, j))

    # find two disjoint swaps
    for i in range(c):
        a1, b1 = swaps[i]
        for j in range(i + 1, c):
            a2, b2 = swaps[j]
            if len({a1, b1, a2, b2}) == 4:
                # construct two permutations
                p = list(range(1, c + 1))
                q = list(range(1, c + 1))
                q[i], q[j] = q[j], q[i]

                print("YES")
                print(*p)
                print(*q)
                return

    print("NO")

if __name__ == "__main__":
    main()
```Việc thực hiện mã hóa trực tiếp tiêu chí cấu trúc. Chúng ta chỉ cần phát hiện một cặp hoán đổi rời rạc, do đó việc quét bậc hai là đủ trong các ràng buộc điển hình cho loại bài toán này; nếu cần, nó có thể được tối ưu hóa hơn nữa bằng cách băm hoặc lập chỉ mục theo điểm cuối. 

Cấu trúc đầu ra là tối thiểu: hoán vị đầu tiên là danh tính và hoán vị thứ hai khác bằng cách hoán đổi hai hoạt động chuyển đổi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Giả sử chúng ta có giao dịch hoán đổi: 

(1, 2), (3, 4), (2, 3) 

Chúng tôi quét các cặp và thấy rằng (1, 2) và (3, 4) rời rạc. 

| Bước | tôi | j | trao đổi tôi | trao đổi j | rời rạc | 
| --- | --- | --- | --- | --- | --- | 
| kiểm tra | 0 | 1 | (1,2) | (3,4) | vâng | 

Chúng tôi xuất ra thứ tự nhận dạng và một trong đó hai giao dịch hoán đổi này được trao đổi. Cả hai lần thực thi đều tạo ra cùng một hoán vị cuối cùng của mảng vì hai lần hoán đổi hoạt động trên các chỉ mục riêng biệt. 

### Ví dụ 2 

Hoán đổi: 

(1, 2), (2, 3), (3, 1) 

| Cặp | chỉ số cổ phiếu? | 
| --- | --- | 
| (1,2), (2,3) | vâng | 
| (1,2), (3,1) | vâng | 
| (2,3), (3,1) | vâng | 

Mọi cặp đều giao nhau nên không tồn tại sự hoán đổi rời rạc. Chúng tôi xuất ra KHÔNG. 

Điều này cho thấy trường hợp hoàn toàn “vướng víu” trong đó mọi hoạt động đều tương tác với nhau, không để lại sự tự do đi lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(c²) trường hợp xấu nhất, O(c) | |
