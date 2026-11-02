---
title: "CF 1076A - Giảm thiểu chuỗi"
description: "Chúng ta được cấp một chuỗi gồm các chữ cái viết thường và chúng ta được phép xóa nhiều nhất một ký tự khỏi chuỗi đó. Sau khi xóa tùy chọn này, chúng tôi nhận được một chuỗi mới và trong số tất cả các kết quả có thể xảy ra (bao gồm cả việc không làm gì), chúng tôi muốn chuỗi nhỏ nhất về mặt từ điển."
date: "2026-06-15T06:47:25+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "strings"]
categories: ["algorithms"]
codeforces_contest: 1076
codeforces_index: "A"
codeforces_contest_name: "Educational Codeforces Round 54 (Rated for Div. 2)"
rating: 1200
weight: 1076
solve_time_s: 88
verified: true
draft: false
---

[CF 1076A - Giảm thiểu chuỗi](https://codeforces.com/problemset/problem/1076/A) 

**Đánh giá:** 1200 
**Tags:** tham lam, dây 
**Thời gian giải:** 1 phút 28s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi gồm các chữ cái viết thường và chúng ta được phép xóa nhiều nhất một ký tự khỏi chuỗi đó. Sau khi xóa tùy chọn này, chúng tôi nhận được một chuỗi mới và trong số tất cả các kết quả có thể xảy ra (bao gồm cả việc không làm gì), chúng tôi muốn chuỗi nhỏ nhất về mặt từ điển. 

Thứ tự từ điển ở đây hoạt động giống như so sánh từ điển: hai chuỗi được so sánh từng ký tự ngay từ đầu và vị trí đầu tiên nơi chúng khác nhau sẽ quyết định chuỗi nào nhỏ hơn. Nếu một chuỗi là tiền tố của chuỗi kia thì chuỗi ngắn hơn sẽ nhỏ hơn. 

Ràng buộc cho phép độ dài chuỗi lên tới 200.000. Điều đó ngay lập tức loại trừ mọi giải pháp thử mọi vị trí xóa một cách độc lập và so sánh các chuỗi một cách ngây thơ, vì điều đó sẽ dẫn đến hành vi O(n²) trong trường hợp xấu nhất. Ngay cả việc quét tuyến tính lặp lại trên mỗi vị trí cũng sẽ quá chậm trong giới hạn chặt chẽ. 

Một vài tình huống khó khăn quan trọng. 

Nếu chuỗi đã không giảm theo nghĩa từ điển khi được xem dưới dạng một chuỗi trong đó các ký tự đầu đã càng nhỏ càng tốt thì việc xóa bất kỳ thứ gì có thể không giúp ích được gì. Ví dụ, trong`"aaa"`, mọi ký tự đều giống hệt nhau nên mọi thao tác xóa đều cho kết quả như nhau. 

Một trường hợp tinh tế hơn là khi lần “thả” đầu tiên vào chuỗi rất muộn. Ví dụ, trong`"abdc"`, loại bỏ một trong hai`'d'`hoặc`'c'`có thể ảnh hưởng đến kết quả từ điển và sự lựa chọn tối ưu phụ thuộc vào nơi xảy ra vi phạm tính đơn điệu đầu tiên. 

Một trường hợp thất bại khác đối với lối suy luận ngây thơ là cho rằng chúng ta phải luôn loại bỏ ký tự đầu tiên lớn hơn ký tự tiếp theo. Vì`"bcaa"`, loại bỏ`'b'`cho`"caa"`, nhưng loại bỏ`'c'`cho`"baa"`, cái nào nhỏ hơn 

Khó khăn cốt lõi là quyết định không mang tính địa phương. Việc xóa sớm hơn trong chuỗi có thể ảnh hưởng đến bất kỳ lợi ích nào đạt được sau này. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực rất đơn giản: thử xóa từng vị trí một lần (cộng với tùy chọn không xóa gì), tạo chuỗi kết quả và so sánh tất cả các kết quả để tìm giá trị tối thiểu. Mỗi công trình xây dựng ứng viên có chi phí O(n) và có O(n) ứng viên, dẫn đến thời gian O(n2). Với n lên tới 200.000 thì điều này hoàn toàn không khả thi. 

Quan sát quan trọng là chúng ta thực sự không cần so sánh các chuỗi ứng cử viên đầy đủ. Chúng ta chỉ cần quyết định việc loại bỏ một ký tự sẽ cải thiện điểm đầu tiên của sự phân kỳ từ điển ở đâu. Điều này có nghĩa là chúng tôi đang tìm kiếm vị trí sớm nhất mà chuỗi không còn ở trạng thái “tối ưu cục bộ”. 

Chính xác hơn, chúng tôi quét từ trái sang phải và tìm chỉ mục đầu tiên trong đó ký tự lớn hơn ký tự tiếp theo. Tại thời điểm đó, việc giữ lại ký tự hiện tại sẽ làm cho tiền tố tệ hơn mức cần thiết, vì chúng ta có thể bỏ qua nó và ngay lập tức hiển thị một ký tự nhỏ hơn trước đó trong chuỗi. Khi chúng tôi loại bỏ ký tự đó, mọi quyết định sau này sẽ không còn ảnh hưởng đến mức tối thiểu về từ điển nữa, bởi vì việc so sánh từ điển đã được quyết định bởi sự khác biệt sớm nhất. 

Nếu không có vị trí như vậy tồn tại thì chuỗi không giảm, nghĩa là mọi ký tự đều nhỏ hơn hoặc bằng ký tự tiếp theo. Trong trường hợp đó, việc loại bỏ ký tự cuối cùng sẽ tạo ra kết quả nhỏ nhất vì nó làm giảm độ dài mà không làm xấu đi thứ tự từ điển. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Duyệt chuỗi từ ký tự đầu tiên đến ký tự cuối cùng thứ hai. 

Chúng tôi so sánh từng ký tự với ký tự tiếp theo vì chỉ có thể cải thiện từ điển khi ký tự lớn hơn đứng trước ký tự nhỏ hơn. 
2. Tìm chỉ số đầu tiên`i`như vậy`s[i] > s[i+1]`. 

Đây là điểm đầu tiên mà chuỗi không còn tối ưu theo thứ tự tiền tố. 
3. Nếu chỉ mục đó tồn tại, hãy xây dựng câu trả lời bằng cách loại bỏ`s[i]`. 

Việc loại bỏ ký tự này sẽ hiển thị một ký tự nhỏ hơn trước đó, điều này đảm bảo tiền tố nhỏ nhất có thể về mặt từ điển. 
4. Nếu không có chỉ mục nào tồn tại thì chuỗi không giảm. 

Trong trường hợp này, việc xóa bất kỳ ký tự bên trong nào cũng không thể cải thiện thứ tự từ điển, vì vậy chúng tôi xóa ký tự cuối cùng để giảm thiểu độ dài. 

### Tại sao nó hoạt động 

Thuật toán dựa trên thực tế là so sánh từ điển được xác định bởi vị trí khác nhau đầu tiên. Chỉ số sớm nhất nơi`s[i] > s[i+1]`là cơ hội đầu tiên để nâng cao vị trí quyết định đó. Việc xóa bất kỳ ký tự nào trước chỉ mục này sẽ chỉ làm cho tiền tố trở nên tồi tệ hơn hoặc không thay đổi, trong khi việc xóa bất kỳ ký tự nào sau nó sẽ không thể ảnh hưởng đến việc so sánh tiền tố đã được xác định. Điều này tạo ra một cấu trúc con tối ưu tham lam: “điểm rơi” đầu tiên xác định đầy đủ điểm xóa tốt nhất có thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    s = input().strip()

    drop = -1
    for i in range(n - 1):
        if s[i] > s[i + 1]:
            drop = i
            break

    if drop == -1:
        print(s[:-1])
    else:
        print(s[:drop] + s[drop + 1:])

if __name__ == "__main__":
    solve()
```Giải pháp đọc chuỗi và quét một lần để tìm vị trí đầu tiên có ký tự lớn hơn ký tự tiếp theo. Chỉ mục đó được lưu trữ dưới dạng`drop`. Nếu không có chỉ mục nào như vậy tồn tại, chúng tôi sẽ xóa ký tự cuối cùng bằng cách cắt`s[:-1]`. 

Một điểm tế nhị là chúng ta vi phạm ngay lần đầu tiên. Điều này rất cần thiết vì những vi phạm sau này sẽ không còn phù hợp nữa một khi tiền tố quan trọng về mặt từ điển đã được sửa. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`"aaa"`| tôi | s[i] | s[i+1] | thả | 
| --- | --- | --- | --- | 
| 0 | một | một | - | 
| 1 | một | một | - | 

Không tìm thấy phần thả nào, vì vậy chúng tôi xóa ký tự cuối cùng. 

Đầu ra trở thành`"aa"`. 

Điều này xác nhận rằng khi tất cả các ký tự đều bằng nhau thì cách tốt nhất chỉ đơn giản là giảm độ dài. 

### Ví dụ 2 

đầu vào:`"abdc"`| tôi | s[i] | s[i+1] | thả | 
| --- | --- | --- | --- | 
| 0 | một | b | - | 
| 1 | b | d | - | 
| 2 | d | c | 2 | 

Chúng tôi dừng lại ở chỉ số 2 vì`d > c`. Đang xóa`d`cho`"abc"`. 

Đầu ra:`"abc"`. 

Điều này chứng tỏ rằng điểm giảm đầu tiên xác định việc xóa tối ưu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chuyển một lần để tìm cặp giảm đầu tiên, cộng với việc cắt O(n) | 
| Không gian | O(n) | Xây dựng chuỗi đầu ra | 

Thuật toán chạy thoải mái trong giới hạn n lên tới 200.000 vì nó chỉ thực hiện một lần quét tuyến tính duy nhất và một lần tái cấu trúc tuyến tính của chuỗi đầu ra. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    old_stdout = sys.stdout
    sys.stdout = output

    def solve():
        n = int(input())
        s = input().strip()

        drop = -1
        for i in range(n - 1):
            if s[i] > s[i + 1]:
                drop = i
                break

        if drop == -1:
            print(s[:-1])
        else:
            print(s[:drop] + s[drop + 1:])

    solve()
    sys.stdout = old_stdout
    return output.getvalue().strip()

# provided samples
assert run("3\naaa\n") == "aa"

# custom cases
assert run("2\nba\n") == "a"        # remove first char
assert run("4\nabcd\n") == "abc"    # monotone increasing
assert run("5\ncbacd\n") == "bacd"  # early drop
assert run("3\nzyx\n") == "yx"      # drop first
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`ba`|`a`| thả một lần khi bắt đầu | 
|`abcd`|`abc`| trường hợp tăng nghiêm ngặt | 
|`cbacd`|`bacd`| quy tắc đảo ngược đầu tiên | 
|`zyx`|`yx`| loại bỏ tối ưu ngay lập tức | 

## Vỏ cạnh 

Đối với một chuỗi tăng đầy đủ như`"abcde"`, thuật toán không bao giờ tìm được vị trí tại đó`s[i] > s[i+1]`, Vì thế`drop`ở lại`-1`. Mã loại bỏ ký tự cuối cùng và trả về`"abcd"`, điều này đúng vì bất kỳ thao tác xóa nội bộ nào cũng sẽ tạo ra một tiền tố lớn hơn về mặt từ điển thay vì chỉ rút ngắn chuỗi. 

Đối với một chuỗi có sự đảo ngược sớm như`"za"`, vòng lặp ngay lập tức phát hiện`z > a`tại chỉ số 0. Loại bỏ chỉ số 0 sẽ tạo ra`"a"`, đây là kết quả nhỏ nhất có thể có vì bất kỳ thao tác nào khác sẽ giữ nguyên`'z'`làm tiền tố và làm xấu đi trật tự từ điển. 

Đối với các chuỗi có ký tự lặp lại như`"bbbbbb"`, không tồn tại sự đảo ngược nên thuật toán sẽ loại bỏ ký tự cuối cùng, tạo ra`"bbbbb"`. Mọi thao tác xóa có thể đều mang lại giá trị từ điển giống nhau và lựa chọn này duy trì tính chính xác trong khi vẫn nhất quán.
