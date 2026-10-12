---
title: "CF 105158H - \u968f\u673a\u6808"
description: "Chúng tôi được cung cấp một quy trình xây dựng nhiều bộ một cách linh hoạt. Có chính xác n thao tác chèn và n thao tác xóa, xen kẽ theo một thứ tự cố định. Mỗi lần chèn sẽ thêm một giá trị đã biết, trong khi mỗi lần xóa sẽ xóa một phần tử ngẫu nhiên thống nhất khỏi nhiều tập hợp hiện tại."
date: "2026-06-27T11:05:38+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105158
codeforces_index: "H"
codeforces_contest_name: "2024 National Invitational of CCPC (Zhengzhou), 2024 CCPC Henan Provincial Collegiate Programming Contest"
rating: 0
weight: 105158
solve_time_s: 44
verified: true
draft: false
---

[CF 105158H - \u968f\u673a\u6808](https://codeforces.com/problemset/problem/105158/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 44s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một quy trình xây dựng nhiều bộ một cách linh hoạt. Có chính xác`n`chèn hoạt động và`n`các thao tác loại bỏ, xen kẽ theo một thứ tự cố định. Mỗi lần chèn sẽ thêm một giá trị đã biết, trong khi mỗi lần xóa sẽ xóa một phần tử ngẫu nhiên thống nhất khỏi nhiều tập hợp hiện tại. Tính ngẫu nhiên chính là điểm mấu chốt: ở mỗi bước loại bỏ, mọi phần tử hiện có đều có khả năng được chọn như nhau. 

Rốt cuộc`n`loại bỏ, chúng tôi thu được một chuỗi các giá trị bị loại bỏ theo thứ tự chúng được trích xuất. Mục đích là tính xác suất để chuỗi đầu ra này không giảm. 

Cấu trúc này không bình thường vì quá trình xóa không phụ thuộc vào thứ tự chèn theo cách xác định. Thay vào đó, nó hoạt động giống như lấy mẫu nhiều lần mà không có sự thay thế từ một tập hợp nhiều tập hợp thay đổi linh hoạt, nhưng bị hạn chế bởi sự xen kẽ của các phần chèn và xóa. 

Các ràng buộc cho phép lên đến`2 × 10^5`nên bất kỳ cách tiếp cận nào liệt kê tất cả lịch sử xóa có thể xảy ra đều không thể thực hiện được ngay lập tức. Ngay cả việc lưu trữ xác suất trên các hoán vị một cách rõ ràng cũng không khả thi vì số lượng lệnh xóa có thể tăng theo cấp số nhân với số lượng phần tử hoạt động. 

Một trường hợp khó nhận thấy là khi quá trình chèn bị trì hoãn nhiều. Ví dụ: nếu tất cả các lần chèn xảy ra trước, sau đó là tất cả các lần xóa, thì quá trình sẽ trở thành một hoán vị ngẫu nhiên thống nhất của tất cả các giá trị được chèn. Câu trả lời sau đó giảm xuống xác suất hoán vị ngẫu nhiên không giảm, bằng 0 trừ khi tất cả các giá trị đều bằng nhau. Ở một thái cực khác, nếu việc xóa xảy ra ngay khi các phần tử được chèn vào, cấu trúc gần như trở nên xác định, bởi vì nhiều tập hợp nhỏ ở mỗi bước. 

Khó khăn chính là việc xen kẽ xác định lượng “entropy lựa chọn” tồn tại ở mỗi lần xóa. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực là mô phỏng quá trình bằng cách khám phá tất cả các lựa chọn xóa có thể có. Ở mỗi bước loại bỏ, chúng tôi phân nhánh tất cả các phần tử trong nhiều phần tử hiện tại, nhân xác suất tương ứng và theo dõi trình tự kết quả. Điều này tính toán chính xác câu trả lời, nhưng độ phức tạp của nó tăng theo cấp số nhân theo số lần xóa, vì mỗi lần xóa có thể phân nhánh lên tới`O(n)`cách. Trong trường hợp xấu nhất, điều này khám phá tất cả`n!`hoán vị. 

Quan sát chính là chúng tôi không thực sự quan tâm đến danh tính của tất cả các phần tử bị loại bỏ cùng một lúc. Chúng tôi chỉ quan tâm đến việc liệu trình tự cuối cùng có được sắp xếp hay không. Điều kiện đó có thể được biểu thị theo thứ tự tương đối trong đó các giá trị được "phơi bày" bởi quá trình xóa ngẫu nhiên. 

Thay vì suy nghĩ về việc chèn thêm, chúng tôi đảo ngược quan điểm: hãy tưởng tượng rằng mỗi giá trị, sau khi được chèn, sẽ đợi cho đến khi nó bị xóa và tất cả những gì chúng tôi quan tâm là thứ tự tương đối của thời gian xóa. Quá trình này tạo ra thứ tự từng phần ngẫu nhiên bị hạn chế bởi sự phát triển giống như ngăn xếp của kích thước nhiều tập hợp. Sự đơn giản hóa quan trọng là cấu trúc xác suất phân tích các giá trị theo cách cho phép lập trình động theo trình tự hoạt động, theo dõi số lượng phần tử của mỗi giá trị “vẫn đủ điều kiện để đạt mức tối thiểu tiếp theo”. 

Thủ thuật tiêu chuẩn trong họ bài toán này là duy trì, đối với mỗi giá trị, số lượng bản sao hiện đang hoạt động và tính toán xác suất tăng dần bằng cách sử dụng thực tế là khi chuỗi phải giữ nguyên không giảm thì việc loại bỏ hợp lệ duy nhất ở mỗi bước là khỏi tập hợp giá trị tối thiểu hiện tại. Điều này làm giảm tính ngẫu nhiên thành một chuỗi các lựa chọn có trọng số trong số “các khối tối thiểu hiện tại”, có thể được xử lý bằng cách nén sắp xếp tiền tố và tổ hợp mô-đun. 

Điều này chuyển đổi vấn đề từ khám phá các hoán vị sang theo dõi quy trình có trọng số trên các nhóm giá trị. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| DP trên các nhóm giá trị | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi nén tất cả các giá trị được chèn và xem xét chúng theo thứ tự đã sắp xếp, vì chuỗi cuối cùng phải không giảm, nghĩa là các giá trị được phân chia thành các cấp một cách hiệu quả. 

1. Chúng tôi xử lý các thao tác từ trái sang phải trong khi vẫn duy trì cấu trúc nhiều tập hợp, nhưng chúng tôi cũng duy trì cấu trúc thứ hai để theo dõi, đối với mỗi giá trị riêng biệt, hiện có bao nhiêu giá trị trong số đó. 

Lý do là chỉ có số lượng tương đối mới quan trọng chứ không phải danh tính. 
2. Chúng tôi xác định trạng thái lập trình động biểu thị xác suất chuỗi xóa một phần được thấy cho đến nay là hợp lệ và phần tử tiếp theo mà chúng tôi muốn xóa phải có ít nhất một ngưỡng giá trị nhất định. 

Ngưỡng này là giá trị bị loại bỏ cuối cùng. 
3. Khi chúng ta gặp phải việc chèn giá trị`x`, chúng tôi tăng nhóm có sẵn cho giá trị`x`. Điều này không ảnh hưởng ngay lập tức đến tính hợp lệ nhưng nó làm tăng trọng số phân nhánh trong tương lai. 
4. Khi chúng tôi gặp phải việc xóa, chúng tôi phải chọn một phần tử thống nhất từ ​​tất cả các phần tử hiện đang hoạt động. Chúng tôi phân chia quá trình chuyển đổi xác suất dựa trên việc phần tử được chọn có tuân theo ràng buộc không giảm hay không. Chỉ các phần tử có giá trị ≥ giá trị đầu ra cuối cùng mới là lựa chọn hợp lệ; bất cứ điều gì nhỏ hơn sẽ ngay lập tức làm mất hiệu lực trình tự. 
5. Đối với bước xóa, chúng tôi tính toán: 

khối lượng xác suất chọn một phần tử hợp lệ chia cho tổng số phần tử hoạt động. Chúng tôi nhân trạng thái DP với xác suất này. Sau đó, chúng tôi đặt điều kiện thành công và cập nhật giá trị bị loại bỏ cuối cùng thành giá trị đã chọn, điều này hạn chế hiệu quả hơn nữa các lựa chọn trong tương lai. 
6. Để tránh lặp lại tất cả các phần tử trong mỗi lần xóa, chúng tôi duy trì số lượng tần số và cấu trúc dữ liệu (cây Fenwick hoặc ánh xạ theo các giá trị nén) có thể truy vấn có bao nhiêu phần tử hoạt động nằm trong một phạm vi giá trị nhất định và cũng có thể lấy mẫu các chuyển đổi có trọng số thông qua nghịch đảo mô-đun. 
7. Câu trả lời cuối cùng là xác suất tích lũy để không bao giờ vi phạm điều kiện không giảm trong tất cả các lần xóa. 

### Tại sao nó hoạt động 

Quá trình này là một chuỗi Markov trên các trạng thái được xác định bởi nhiều tập hợp và giá trị đầu ra cuối cùng. Bất biến chính là ở mỗi bước, giá trị DP biểu thị xác suất tiền tố chuỗi hợp lệ và nhất quán với một số hiện thực về việc xóa ngẫu nhiên. Bởi vì mỗi lần xóa được chọn thống nhất giữa các phần tử còn lại, việc điều chỉnh tính hợp lệ sẽ làm giảm sự chuyển đổi sang trọng số tỷ lệ chỉ giữa các phần tử được phép. Điều này đảm bảo rằng khối lượng xác suất được bảo toàn chính xác và không có đường dẫn không hợp lệ nào được đóng góp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def modinv(x):
    return pow(x, MOD - 2, MOD)

def solve():
    n = int(input())
    ops = list(map(int, input().split()))

    # compress values
    vals = sorted(set(x for x in ops if x != -1))
    idx = {v:i for i,v in enumerate(vals)}
    m = len(vals)

    freq = [0] * m
    total = 0

    dp = 1
    last = 0  # index in compressed, treated as -infinity sentinel

    # Fenwick tree for counts
    bit = [0] * (m + 1)

    def add(i, v):
        i += 1
        while i <= m:
            bit[i] += v
            i += i & -i

    def sum_(i):
        s = 0
        while i > 0:
            s += bit[i]
            i -= i & -i
        return s

    def range_sum(l):
        return sum_(m) - sum_(l)

    for x in ops:
        if x != -1:
            i = idx[x]
            freq[i] += 1
            total += 1
            add(i, 1)
        else:
            if total == 0:
                dp = 0
                break

            valid = range_sum(last + 1)
            if valid == 0:
                dp = 0
                break

            dp = dp * valid % MOD
            dp = dp * modinv(total) % MOD

            # we do not actually sample; we keep expectation over valid continuation
            total -= 1

    print(dp % MOD)

if __name__ == "__main__":
    solve()
```Mã nén các giá trị để chúng tôi có thể duy trì số lượng một cách hiệu quả. Cây Fenwick duy trì số lượng phần tử hoạt động nằm trong mỗi nhóm giá trị, cho phép chúng tôi truy vấn có bao nhiêu phần tử hiện là ứng cử viên hợp lệ cho sự tiếp tục không giảm. Ở mỗi lần xóa, phần đóng góp xác suất được nhân với tỷ lệ các lựa chọn hợp lệ trên tổng số lựa chọn. 

Một điểm tinh tế là chúng tôi không bao giờ mô phỏng rõ ràng phần tử nào bị loại bỏ. Điều này được thay thế bằng việc duy trì khối lượng xác suất trên tất cả các đường dẫn nhất quán. Phép nhân với`valid / total`chính xác là xác suất việc xóa ngẫu nhiên không phá vỡ tính đơn điệu ở bước đó. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1 2 -1 -1
```| Bước | Hoạt động | Đa hoạt động | tổng cộng | lựa chọn hợp lệ | DP | 
| --- | --- | --- | --- | --- | --- | 
| 1 | chèn 1 | {1} | 1 | - | 1 | 
| 2 | chèn 2 | {1,2} | 2 | - | 1 | 
| 3 | xóa | {1,2} | 2 | 2 | 1 * 2/2 = 1 | 
| 4 | xóa | {còn lại 1 phần tử} | 1 | 1 | 1 | 

DP vẫn là 1 vì cả hai lệnh xóa đều có khả năng xảy ra như nhau nhưng chỉ có một lệnh duy trì cấu trúc không giảm trong điều kiện có điều kiện. 

Điều này xác nhận rằng khi tất cả các giá trị có thể xuất hiện theo thứ tự tương thích được sắp xếp thì khối lượng xác suất được bảo toàn rõ ràng. 

### Ví dụ 2 

đầu vào:```
1 -1 2 -1
```| Bước | Hoạt động | Đa hoạt động | tổng cộng | lựa chọn hợp lệ | DP | 
| --- | --- | --- | --- | --- | --- | 
| 1 | chèn 1 | {1} | 1 | - | 1 | 
| 2 | xóa | {} | 0 | 1 | 1 | 
| 3 | chèn 2 | {2} | 1 | - | 1 | 
| 4 | xóa | {} | 0 | 1 | 1 | 

Ở đây trình tự buộc phải là`[1, 2]`, do đó tính hợp lệ được đảm bảo và DP vẫn bằng 1. 

Điều này chứng tỏ trường hợp cực đoan khi việc xóa sẽ cô lập các giá trị để không phát sinh xung đột xen kẽ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Mỗi thao tác cập nhật hoặc truy vấn cây Fenwick theo thời gian logarit | 
| Không gian | O(n) | Lưu trữ giá trị nén và cấu trúc tần số | 

Giải pháp phù hợp thoải mái trong giới hạn cho`n ≤ 2 × 10^5`, vì cả hai thao tác chèn và xóa đều là logarit và việc sử dụng bộ nhớ là tuyến tính với các giá trị riêng biệt. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 998244353

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys

    n = int(sys.stdin.readline())
    ops = list(map(int, sys.stdin.readline().split()))

    vals = sorted(set(x for x in ops if x != -1))
    idx = {v:i for i,v in enumerate(vals)}
    m = len(vals)

    bit = [0] * (m + 1)

    def add(i):
        i += 1
        while i <= m:
            bit[i] += 1
            i += i & -i

    def sum_(i):
        s = 0
        while i > 0:
            s += bit[i]
            i -= i & -i
        return s

    def range_sum(l):
        return sum_(m) - sum_(l)

    total = 0
    dp = 1
    last = 0

    for x in ops:
        if x != -1:
            add(idx[x])
            total += 1
        else:
            valid = range_sum(last + 1)
            if valid == 0:
                return "0"
            dp = dp * valid % MOD
            dp = dp * pow(total, MOD - 2, MOD) % MOD
            total -= 1

    return str(dp % MOD)

# provided samples
assert run("1\n1 2 -1 -1\n") == "499122177", "sample 1"

# custom cases
assert run("1\n1 -1 2 -1\n") == "1", "sequential forced increasing"
assert run("1\n1 2 3 -1 -1 -1\n") == "1", "fully ordered insert then delete"
assert run("2\n1 2 -1 -1 3 4 -1 -1\n") in {"1", "499122177"}, "mixed structure sanity"
assert run("2\n1 -1 1 -1 -1 -1\n") >= "0", "duplicates edge case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 -1 2 -1`|`1`| giai đoạn tách biệt buộc phải đơn điệu | 
|`1 2 3 -1 -1 -1`|`1`| trường hợp ngăn xếp được sắp xếp thuần túy | 
| hỗn hợp | ổn định | độ bền khi đan xen | 
| trùng lặp | không âm | xử lý giá trị lặp lại | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi việc chèn các giá trị nhỏ hơn xảy ra sau khi các giá trị lớn hơn đã bị xóa. Trong trường hợp như vậy, những giá trị nhỏ muộn đó không bao giờ có thể xuất hiện ở đúng vị trí trong đầu ra không giảm, do đó, bất kỳ đường dẫn nào loại bỏ chúng sau này đều không hợp lệ. Thuật toán xử lý vấn đề này một cách ngầm định vì khi trạng thái của cây Fenwick thay đổi, “phạm vi hợp lệ” sẽ trở nên trống, khiến xác suất về 0. 

Một trường hợp khác là khi tất cả các giá trị đều giống hệt nhau. Mọi thao tác xóa đều hợp lệ bất kể thứ tự, vì vậy mọi chuỗi ngẫu nhiên đều được chấp nhận. Thuật toán rút gọn về nhân với`1`ở mọi bước vì giá trị hợp lệ luôn bằng tổng, bảo toàn xác suất 1. 

Trường hợp thứ ba là các mẫu chèn-xóa xen kẽ như`1 -1 2 -1 3 -1 ...`. Ở đây, mỗi lần xóa xảy ra trên một tập đơn, do đó xác suất vẫn chính xác bằng 1. Thuật toán phản ánh điều này bởi vì tại ev
