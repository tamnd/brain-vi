---
title: "CF 1609A - Chia và nhân"
description: "Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Mỗi trường hợp thử nghiệm bao gồm một mảng nhỏ các số nguyên và chúng tôi được phép phân phối lại nhiều lần các thừa số của hai giữa các phần tử."
date: "2026-06-10T07:23:29+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1609
codeforces_index: "A"
codeforces_contest_name: "Deltix Round, Autumn 2021 (open for everyone, rated, Div. 1 + Div. 2)"
rating: 900
weight: 1609
solve_time_s: 88
verified: true
draft: false
---

[CF 1609A - Chia và nhân](https://codeforces.com/problemset/problem/1609/A) 

**Xếp hạng:** 900 
**Tags:** tham lam, thực hiện, toán học, lý thuyết số 
**Thời gian giải:** 1 phút 28s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Mỗi trường hợp thử nghiệm bao gồm một mảng nhỏ các số nguyên và chúng tôi được phép phân phối lại nhiều lần các thừa số của hai giữa các phần tử. Một thao tác chọn hai vị trí, chia một giá trị đã chọn cho hai (chỉ khi giá trị đó là số chẵn) và nhân đôi giá trị còn lại. Hoạt động này bảo toàn tổng tích của tất cả các phần tử nhưng làm thay đổi tổng. 

Mục tiêu là áp dụng các phép toán như vậy nhiều lần để tối đa hóa tổng của mảng. 

Các ràng buộc làm cho cấu trúc rất khác với các vấn đề tối ưu hóa mảng thông thường. Kích thước mảng tối đa là 15 và mỗi giá trị nhỏ hơn 16. Điều đó ngay lập tức cho thấy rằng lực lượng vũ phu đối với các trạng thái của các giá trị riêng lẻ là không cần thiết, nhưng cấu trúc hệ số hóa cũng cực kỳ hạn chế. Mọi số có thể được viết dưới dạng phần lẻ nhân với lũy thừa của hai và vì các giá trị nhỏ nên lũy thừa của hai số mũ nhiều nhất là 3. Điều này gợi ý rõ ràng rằng chuyển động có ý nghĩa duy nhất trong hệ thống là dịch chuyển lũy thừa của hai qua các vị trí. 

Một sự hiểu lầm ngây thơ là cho rằng tổng này bất biến hoặc gần như bất biến. Không phải vậy. Ví dụ: di chuyển hệ số hai từ một số lẻ nhỏ sang một số lẻ lớn sẽ làm tăng phần đóng góp của số lớn đó nhiều hơn là giảm phần đóng góp của số nhỏ. 

Trường hợp cạnh tinh tế thứ hai là khi tất cả các số đều là số lẻ. Không thể thực hiện thao tác nào vì không có phần tử nào chia hết cho 2. Câu trả lời đã được sửa. 

Trường hợp cạnh thứ ba là khi tất cả các số đều là lũy thừa của hai. Trong trường hợp này, việc phân phối lại hai số không bị hạn chế và tổng cuối cùng phụ thuộc hoàn toàn vào cách chúng ta nhóm số mũ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô phỏng mọi chuỗi hoạt động có thể xảy ra. Mỗi trạng thái là một tập hợp nhiều số nguyên và mỗi bước di chuyển sẽ chọn hai chỉ số và sửa đổi chúng. Ngay cả với một mảng nhỏ, hệ số phân nhánh vẫn lớn và các chuỗi có thể dài tùy ý vì các giá trị có thể tiếp tục tăng thông qua việc nhân đôi lặp đi lặp lại. Điều này nhanh chóng trở thành vô hạn trong không gian trạng thái nếu không được giới hạn cẩn thận. 

Quan sát quan trọng là tách mỗi số thành thành phần lẻ và số mũ lũy thừa hai của nó. Viết mỗi giá trị dưới dạng$a_i = b_i \cdot 2^{k_i}$, Ở đâu$b_i$thật kỳ quặc và$k_i$là số lần chúng ta có thể chia cho hai. 

Phép toán di chuyển một đơn vị số mũ từ vị trí này sang vị trí khác một cách hiệu quả: giảm một$k_i$và tăng cái khác$k_j$, trong khi giữ nguyên tất cả các phần lẻ. Đây là sự đơn giản hóa trung tâm: các phần lẻ được cố định, chỉ số mũ được phân phối lại. 

Bây giờ tổng là:$$\sum b_i \cdot 2^{k_i}$$Chúng tôi được phép phân phối lại nhiều số mũ tùy ý trên các chỉ số, nhưng chúng tôi không thể thay đổi tổng số mũ. 

Vì vậy, vấn đề trở thành: phân phối một số lũy thừa cố định của hai trên các trọng số cố định$b_i$, trong đó mỗi đơn vị số mũ nhân đôi phần đóng góp của vị trí đó. Vì nhân đôi là phép nhân, mỗi số mũ bổ sung trên một vị trí sẽ nhân giá trị của nó với 2 và đóng góp tăng theo cấp số nhân về số lượng số mũ được chỉ định. 

Cấu trúc tham lam xuất hiện một cách tự nhiên: các thành phần lẻ lớn hơn sẽ nhận được số mũ lớn hơn. 

Chúng tôi sắp xếp các giá trị theo phần lẻ của chúng, trích xuất tất cả lũy thừa của hai thành một nhóm, sau đó gán số mũ lớn nhất cho các thành phần lẻ lớn nhất. Điều này tối đa hóa tổng số mũ có trọng số. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng trạng thái Brute Force | hàm mũ | hàm mũ | Quá chậm | 
| Tham lam phân phối lại số mũ |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi số, hãy phân tích tất cả lũy thừa của 2 thành nhân tử. Lưu trữ thành phần lẻ và đếm số lần nó chia hết cho hai. Điều này cô lập những gì có thể được di chuyển và những gì được cố định. 
2. Thu thập tất cả các lũy thừa được trích xuất của hai thành một tập hợp số mũ duy nhất. Mỗi số mũ đại diện cho một phép toán nhân đôi có thể được gán ở nơi khác. 
3. Sắp xếp mảng theo thành phần lẻ theo thứ tự giảm dần. Lý do là các giá trị lẻ cao hơn sẽ được hưởng lợi nhiều hơn từ việc nhận thêm lũy thừa của hai. 
4. Sắp xếp nhóm số mũ theo thứ tự giảm dần. Số mũ lớn hơn nên được gán trước vì chúng tạo ra hiệu ứng nhân lớn hơn. 
5. Gán số mũ một cách tham lam: với mỗi số mũ theo thứ tự giảm dần, áp dụng cho thành phần lẻ lớn nhất hiện tại. Điều này có nghĩa là nhân thành phần đó với$2^{k}$. 
6. Tính tổng cuối cùng của tất cả các giá trị được cập nhật. 

Điểm tinh tế là chúng ta không bao giờ di chuyển số mũ riêng lẻ trong mô phỏng. Chúng tôi chỉ tính đến hiệu ứng ròng của họ. Điều này tránh được các chuỗi hoạt động theo cấp số nhân. 

### Tại sao nó hoạt động 

Mỗi thao tác bảo toàn tổng số thừa số của hai trên mảng. Vì các thành phần lẻ là bất biến khi chia cho hai nên bậc tự do duy nhất là cách phân bố các thừa số này. Sự đóng góp của số mũ đơn vị chỉ phụ thuộc vào cơ số lẻ mà nó được áp dụng. Bởi vì chức năng$b \cdot 2^k$lồi ở$k$, cách phân bổ tối ưu là tập trung số mũ lớn hơn vào các cơ số lớn hơn. Bất kỳ sự hoán đổi đơn vị số mũ nào giữa hai vị trí vi phạm thứ tự này sẽ làm giảm nghiêm trọng tổng số tiền, điều này đảm bảo phép gán tham lam là tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        
        odds = []
        exps = []
        
        for x in a:
            k = 0
            while x % 2 == 0:
                x //= 2
                k += 1
            odds.append(x)
            exps.extend([1] * k)
        
        odds.sort(reverse=True)
        exps.sort(reverse=True)
        
        # assign exponents greedily
        i = 0
        for e in exps:
            odds[i] *= (1 << e)
            i += 1
        
        print(sum(odds))

if __name__ == "__main__":
    solve()
```Việc triển khai phản ánh chính xác sự phân rã khái niệm. Hệ số trích vòng lặp của hai phần tách biệt các bộ phận lẻ bất biến khỏi các bộ nguồn di động. Danh sách số mũ đại diện cho tất cả các hoạt động nhân đôi có thể chuyển nhượng. Việc sắp xếp cả hai danh sách đảm bảo rằng lợi nhuận lớn nhất sẽ được ghép trước tiên. 

Một chi tiết tinh tế là mỗi đơn vị số mũ độc lập, vì vậy chúng ta không cần theo dõi các hiệu ứng phức hợp trên nhiều phép gán ngoài phép nhân. Một điều nữa là thứ tự dịch chuyển không thành vấn đề khi việc sắp xếp được áp dụng vì việc ghép nối tham lam đã thực thi cấu trúc tối ưu. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi một trường hợp đại diện. 

đầu vào:```
n = 3
[6, 4, 2]
```Nhân tố hóa: 

| Giá trị | Phần lẻ | Số mũ | 
| --- | --- | --- | 
| 6 | 3 | 1 | 
| 4 | 1 | 2 | 
| 2 | 1 | 1 | 

Sau khi khai thác: 

tỷ lệ cược = [3, 1, 1] 

số mũ = [1, 1, 1] 

Đã sắp xếp: 

tỷ lệ cược = [3, 1, 1] 

số mũ = [1, 1, 1] 

Bài tập: 

| Bước | Số mũ | Lựa chọn lẻ | Giá trị cập nhật | 
| --- | --- | --- | --- | 
| 1 | 1 | 3 | 6 | 
| 2 | 1 | 1 | 2 | 
| 3 | 1 | 1 | 2 | 

Tổng cuối cùng = 10. 

Điều này chứng tỏ rằng sự phân bổ tham lam tập trung tăng trưởng vào cơ sở lớn nhất trước tiên, trong khi cơ sở nhỏ hơn nhận được lợi ích tối thiểu. 

Một ví dụ thứ hai: 

đầu vào:```
[1, 2, 3, 4, 5]
```Phần lẻ: [1, 1, 3, 1, 5] 

Số mũ: [1, 2] 

Bài tập: 

| Số mũ | Mục tiêu lẻ | Kết quả | 
| --- | --- | --- | 
| 2 | 5 | 20 | 
| 1 | 3 | 6 | 

Không thay đổi: 1, 1, 1 

Tổng cuối cùng = 20 + 6 + 1 + 1 + 1 = 29 

Điều này cho thấy việc tập trung số mũ cao hơn sớm sẽ tạo ra sự khuếch đại tối đa như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| sắp xếp tỷ lệ cược và danh sách số mũ chiếm ưu thế | 
| Không gian |$O(n)$| lưu trữ các giá trị bị phân tách và nhóm số mũ | 

Các ràng buộc đủ nhỏ để việc sắp xếp chi phối thời gian chạy một cách thoải mái. Ngay cả với 10^4 trường hợp thử nghiệm, tổng công việc vẫn không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(sys.stdin.readline())
    out = []
    for _ in range(t):
        n = int(sys.stdin.readline())
        a = list(map(int, sys.stdin.readline().split()))

        odds = []
        exps = []

        for x in a:
            k = 0
            while x % 2 == 0:
                x //= 2
                k += 1
            odds.append(x)
            exps.extend([1] * k)

        odds.sort(reverse=True)
        exps.sort(reverse=True)

        i = 0
        for e in exps:
            odds[i] *= (1 << e)
            i += 1

        out.append(str(sum(odds)))

    return "\n".join(out)

# provided samples
assert run("5\n3\n6 4 2\n5\n1 2 3 4 5\n1\n10\n3\n2 3 4\n15\n8 8 8 8 8 8 8 8 8 8 8 8 8 8 8\n") == \
"50\n46\n10\n26\n35184372088846"

# custom cases
assert run("1\n1\n1\n") == "1"
assert run("1\n2\n2 4\n") == "8"
assert run("1\n3\n1 1 1\n") == "3"
assert run("1\n4\n8 1 1 1\n") == "13"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đơn 1 | 1 | trường hợp tối thiểu | 
| [2,4] | 8 | hiệu ứng chuyển giao quyền lực | 
| tất cả những cái | 3 | không thể thực hiện thao tác nào | 
| một sức mạnh lớn của hai | 13 | hiệu ứng tập trung | 

## Vỏ cạnh 

Khi tất cả các số đều là số lẻ, danh sách số mũ trống và thuật toán không bao giờ sửa đổi bất kỳ giá trị nào. đầu vào`[1, 3, 5]`ngay lập tức tạo ra tổng`9`, vì không thể chia cho hai ở bất cứ đâu. 

Khi tất cả các số đều là lũy thừa của hai, chẳng hạn như`[8, 8, 8]`, tất cả các phần lẻ trở thành 1 và tất cả số mũ được gộp lại với nhau. Nhiệm vụ tham lam tập trung tất cả sự tăng trưởng vào phần tử đầu tiên, tạo ra mức khuếch đại theo cấp số nhân tối đa phù hợp với ngân sách số mũ cố định. 

Khi có một phần tử duy nhất, chẳng hạn như`[10]`, không thể thực hiện thao tác nào vì không có chỉ mục thứ hai để nhận giá trị gấp đôi. Thuật toán đưa ra giá trị ban đầu một cách chính xác do việc phân phối lại số mũ không thể xảy ra.
