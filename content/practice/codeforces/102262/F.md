---
title: "CF 102262F - \u0422\u0440\u0430\u043d\u0441\u0444\u043e\u0440\u043c\u0430\u0446\u0438\u044f \u0434\u0438\u0440\u0435\u043a\u0442\u043e\u0440\u0438\u0438"
description: "Chúng tôi có hai ảnh chụp nhanh của cùng một cây thư mục, trạng thái ban đầu A và trạng thái cuối cùng B. Mọi đối tượng được liệt kê đều là một thư mục, được nhận dạng bằng dấu / hoặc một tệp có hàm băm liên quan. Bản thân thư mục gốc là ẩn và không xuất hiện trong đầu vào."
date: "2026-08-17T20:22:27+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102262
codeforces_index: "F"
codeforces_contest_name: "\u0427\u0435\u043c\u043f\u0438\u043e\u043d\u0430\u0442 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e - \u0444\u0438\u043d\u0430\u043b (\u042f\u043d\u0434\u0435\u043a\u0441)"
rating: 0
weight: 102262
solve_time_s: 148
verified: true
draft: false
---

[CF 102262F - \u0422\u0440\u0430\u043d\u0441\u0444\u043e\u0440\u043c\u0430\u0446\u0438\u044f \u0434\u0438\u0440\u0435\u043a\u0442\u043e\u0440\u0438\u0438](https://codeforces.com/problemset/problem/102262/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 28s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có hai ảnh chụp nhanh của cùng một cây thư mục, trạng thái ban đầu A và trạng thái cuối cùng B. Mọi đối tượng được liệt kê đều là một thư mục, được nhận dạng bằng dấu sau`/`hoặc một tệp có hàm băm liên quan. Bản thân thư mục gốc là ẩn và không xuất hiện trong đầu vào. 

Các hoạt động được phép là hạn chế có chủ ý. Một thư mục chỉ có thể được tạo khi thư mục mẹ của nó đã tồn tại và chỉ có thể xóa nó khi nó trống. Một tập tin không thể được tạo từ đầu. Cách duy nhất để có được tên tệp mới là tạo một liên kết cứng đến một tệp đã tồn tại và do đó, tên mới có cùng hàm băm với tên nguồn. Các liên kết cứng hiện có có thể được loại bỏ bằng`unlink`. 

Đầu vào mang lại nhiều nhất`10^4`các đối tượng trong mỗi ảnh chụp nhanh, do đó quá trình quét bậc hai thực hiện tối đa`10^8`so sánh đối tượng. Như vậy đã là quá nhiều so với giới hạn 2 giây trong Python và việc so sánh các chuỗi đường dẫn có thể khiến mỗi so sánh trở thành không cố định. Về cơ bản chúng ta cần xử lý tuyến tính hoặc gần tuyến tính. Đường dẫn và độ dài băm tối đa chỉ là 256, vì vậy việc băm và sắp xếp các chuỗi có kích thước này là thực tế. 

Một số trường hợp có thể đánh lừa việc thực hiện bất cẩn. Nếu một tệp nguồn nằm bên trong một thư mục mà cuối cùng phải biến mất thì nguồn đó không thể bị xóa trước khi tất cả các liên kết cứng cần thiết được tạo. Ví dụ,```
1 1
/old/x h
/new/x h
```cần hai thao tác,`link /old/x /new/x`theo sau là`unlink /old/x`. Đang xóa`/old/x`đầu tiên sẽ làm cho liên kết được yêu cầu không thể thực hiện được. 

Các thư mục lồng nhau tạo ra ràng buộc về thứ tự tương tự. Vì```
2 2
/old/
/old/x/
/new/
/new/x/
```mức tối thiểu chính xác là hai thao tác,`mkdir /new/`chỉ là đủ nếu`/new/x/`được biểu diễn trực tiếp dưới dạng thư mục mới thứ hai. Chính xác hơn, với bốn mục đã cho,`/old/`Và`/new/`là những thư mục duy nhất, vì vậy đầu ra là`mkdir /new/`theo sau là`rmdir /old/`. Việc triển khai bất cẩn loại bỏ các thư mục cũ trước khi xử lý nội dung của chúng có thể bị lỗi trên phiên bản sâu hơn như```
3 3
/old/
/old/x/
/old/x/f
/new/
/new/x/
/new/x/g
```nơi cây con cũ phải được làm trống từ dưới lên. 

Trường hợp cạnh thứ ba là một số tệp có cùng hàm băm. Giả định```
2 2
/a h
/b h
/c h
/d h
```Một tệp gốc có thể được sử dụng làm nguồn cho cả hai liên kết cứng mới. Tối thiểu là bốn thao tác, hai`link`hoạt động và hai`unlink`hoạt động. Không có lý do gì để tìm kiếm sự trùng khớp một-một giữa các tệp cũ và mới. 

Cuối cùng, không được phép chạm vào một tệp không thay đổi ngay cả khi một tệp khác có cùng hàm băm đang được di chuyển. Vì```
1 1
/a h
/a h
```câu trả lời đơn giản là`0`. Tên chung đã biểu thị liên kết cứng chính xác và hàm băm của nó được đảm bảo khớp. 

## Phương pháp tiếp cận 

Một giải pháp trực tiếp có thể liên tục tìm kiếm một ảnh chụp nhanh cho mọi đối tượng trong ảnh chụp nhanh khác. Đối với mọi đường dẫn trong A, chúng tôi có thể quét B để quyết định xem nó có còn tồn tại hay không, sau đó thực hiện một lần quét khác để khớp với các giá trị băm của tệp. Điều này đúng vì mọi thao tác được yêu cầu đều có thể được rút ra sau khi tìm thấy rõ ràng đối tượng tương ứng, nhưng chỉ riêng giai đoạn so sánh đầu tiên mới có thể yêu cầu`n * m = 10^8`so sánh đường dẫn khi cả hai ảnh chụp nhanh đều chứa`10^4`đồ vật. Với đường dẫn lên tới 256 ký tự, đây là công việc nhiều hơn mức chúng ta cần. 

Cách tiếp cận bạo lực hoạt động vì danh tính của một đối tượng là đường dẫn hoàn chỉnh của nó. Quan sát hữu ích là các đường dẫn vốn đã là duy nhất và câu lệnh đảm bảo rằng tệp trong một ảnh chụp nhanh không bao giờ có cùng đường dẫn với thư mục trong ảnh chụp nhanh khác. Chúng ta có thể đặt tất cả các đối tượng vào bảng băm và phân loại chúng ngay lập tức theo đường dẫn chính xác. 

Khi đường dẫn được phân loại, số lượng thao tác trên tệp sẽ được cố định. Mọi tệp chỉ tồn tại trong A cuối cùng phải bị xóa, vì vậy mỗi tệp như vậy tốn ít nhất một`unlink`. Mọi tệp chỉ tồn tại trong B phải được tạo dưới dạng liên kết cứng, vì vậy mỗi tệp như vậy có giá ít nhất một`link`. Hai thao tác luôn đủ: tạo mọi liên kết cứng mục tiêu được yêu cầu trong khi vẫn tồn tại một nguồn thích hợp, sau đó xóa tất cả các tên tệp lỗi thời. 

Hàm băm chỉ cần thiết để chọn nguồn cho liên kết cứng mới. Đối với mỗi hàm băm, chúng tôi nhớ một tệp từ A có hàm băm đó. Nếu hàm băm xảy ra trong một tệp không thay đổi, việc ưu tiên tệp đó làm nguồn sẽ thuận tiện vì nó sẽ không bao giờ bị xóa. Mặt khác, tệp chỉ A có hàm băm đó có thể đóng vai trò là nguồn tạm thời. Chúng tôi tạo tất cả các liên kết cứng mới trước khi xóa bất kỳ tệp chỉ A nào, do đó, ngay cả một nguồn được lên lịch xóa vẫn có sẵn đủ lâu. 

Các thư mục có cùng giới hạn dưới cố định. Mỗi thư mục chỉ có trong B yêu cầu một`mkdir`và mọi thư mục chỉ có trong A đều yêu cầu một`rmdir`. Khó khăn duy nhất là đặt hàng. Các thư mục mới phải được tạo từ nông đến sâu, trong khi các thư mục cũ phải được loại bỏ từ sâu đến cạn. Khi tất cả các thư mục mới tồn tại, mọi tệp mục tiêu đều có cha mẹ hợp lệ và khi tất cả các tệp lỗi thời bị xóa, mọi thư mục cũ cuối cùng có thể trở nên trống. 

Do đó, số lượng thao tác tối thiểu chính xác là số lượng đối tượng có đường đi khác nhau giữa A và B. Thuật toán chỉ phải tìm thứ tự hợp pháp cho các thao tác không thể tránh khỏi đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nm · L) | O(n + m) | Quá chậm | 
| Tối ưu | O((n + m) log(n + m) · L) | O(n + m) | Đã chấp nhận | 

Đây`L`là độ dài đường dẫn tối đa, nhiều nhất là 256. Với độ dài đường dẫn bị chặn, điều này có hiệu quả`O((n + m) log(n + m))`. 

## Hướng dẫn thuật toán 

1. Đọc tất cả các đối tượng của A và B và tách các tệp khỏi thư mục. Lưu trữ tập tin dưới dạng`path -> hash`và các thư mục dưới dạng một tập hợp các đường dẫn. Đồng thời, hãy nhớ một tệp A cho mỗi hàm băm. 
2. Tìm các thư mục cần tạo bằng cách lấy`B_dirs - A_dirs`. Sắp xếp chúng bằng cách tăng độ sâu thư mục và sử dụng đường dẫn làm khóa phụ. Thư mục gốc phải tồn tại trước thư mục con của nó, vì vậy mọi thư mục mới được tạo sẽ có thư mục gốc hợp lệ khi hoạt động của nó được phát ra. 
3. Tìm các thư mục cần xóa bằng cách lấy`A_dirs - B_dirs`. Sắp xếp chúng theo độ sâu giảm dần. Một đứa trẻ phải biến mất trước khi cha mẹ của nó trở nên trống rỗng, vì vậy mệnh lệnh này khiến mọi`rmdir`hợp pháp. 
4. Tìm các tệp chỉ xuất hiện trong B. Với mỗi tệp như vậy, hãy tra cứu hàm băm của nó trong bản đồ nguồn được xây dựng từ A và phát ra một`link source target`hoạt động. Tất cả các thư mục đích đã được tạo và tất cả các tệp nguồn từ A vẫn còn tồn tại vì không có`unlink`đã xảy ra chưa. 
5. Tìm các tệp chỉ xuất hiện trong A và phát ra một`unlink`hoạt động của từng người. Tại thời điểm này, tất cả các liên kết cứng mới đã được tạo, do đó, ngay cả một tệp lỗi thời được sử dụng làm nguồn cũng có thể bị xóa một cách an toàn. 
6. Phát ra`rmdir`hoạt động cho các thư mục cũ theo thứ tự độ sâu giảm dần. Tất cả các tệp lỗi thời đã biến mất, vì vậy các thư mục hiện có thể trống. 
7. In tổng số thao tác được phát ra theo sau chính các thao tác đó. Các file và thư mục chung không bao giờ xuất hiện vì chúng đã có đúng trạng thái được yêu cầu. 

### Tại sao nó hoạt động 

Hãy xem xét mọi con đường một cách độc lập. Một đường dẫn chung có cùng loại đối tượng trong cả hai ảnh chụp nhanh và một tệp chung có cùng hàm băm, do đó, việc chạm vào nó không thể làm giảm số lượng thao tác xuống dưới 0. Một tập tin chỉ tồn tại trong B cần ít nhất một`link`, trong khi một tệp chỉ tồn tại trong A cần ít nhất một`unlink`. Thuật toán của chúng tôi thực hiện chính xác những hoạt động đó và tạo ra mọi liên kết mới trước khi xóa bất kỳ nguồn nào có thể có, vì vậy tất cả chúng đều hợp pháp. 

Đối số tương tự áp dụng cho các thư mục. Mỗi thư mục chỉ B cần một`mkdir`và mọi thư mục chỉ có A đều cần một`rmdir`. Việc sắp xếp việc tạo theo độ sâu đảm bảo rằng cha mẹ tồn tại trước tiên. Việc sắp xếp xóa theo độ sâu ngược lại đảm bảo rằng các phần tử con sẽ biến mất trước tiên. Do đó, mọi thao tác được phát ra đều hợp pháp và số lượng thao tác đạt đến giới hạn dưới không thể tránh khỏi, làm cho chuỗi trở nên tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())

    a_files = {}
    a_dirs = set()

    for _ in range(n):
        parts = input().split()
        path = parts[0]

        if path.endswith('/'):
            a_dirs.add(path)
        else:
            a_files[path] = parts[1]

    b_files = {}
    b_dirs = set()

    for _ in range(m):
        parts = input().split()
        path = parts[0]

        if path.endswith('/'):
            b_dirs.add(path)
        else:
            b_files[path] = parts[1]

    common_files = a_files.keys() & b_files.keys()

    source_by_hash = {}

    # Prefer files that survive in B as sources.
    for path in common_files:
        source_by_hash[a_files[path]] = path

    # If a hash has no surviving source, an obsolete A-file can be used
    # until all required links have been created.
    for path, h in a_files.items():
        if h not in source_by_hash:
            source_by_hash[h] = path

    add_dirs = sorted(
        b_dirs - a_dirs,
        key=lambda p: (p.count('/'), p)
    )

    remove_dirs = sorted(
        a_dirs - b_dirs,
        key=lambda p: (-p.count('/'), p)
    )

    add_files = sorted(
        b_files.keys() - a_files.keys()
    )

    remove_files = sorted(
        a_files.keys() - b_files.keys()
    )

    operations = []

    for path in add_dirs:
        operations.append(f"mkdir {path}")

    for target in add_files:
        source = source_by_hash[b_files[target]]
        operations.append(f"link {source} {target}")

    for path in remove_files:
        operations.append(f"unlink {path}")

    for path in remove_dirs:
        operations.append(f"rmdir {path}")

    out = [str(len(operations))]
    out.extend(operations)
    sys.stdout.write('\n'.join(out) + '\n')

if __name__ == "__main__":
    solve()
```Hai từ điển và bộ đầu tiên đại diện trực tiếp cho hai ảnh chụp nhanh. Việc sử dụng đường dẫn đầy đủ làm khóa sẽ giúp việc kiểm tra xem một đối tượng có phổ biến hay không là một thao tác O(1) trung bình. 

các`source_by_hash`bản đồ nắm bắt thông tin duy nhất cần thiết cho các liên kết cứng. Một tệp chung được ưu tiên làm nguồn vì nó sẽ tồn tại cho đến cuối cùng. Nếu không có tệp chung nào có hàm băm được yêu cầu thì tệp chỉ A sẽ trở thành nguồn. Một tập tin như vậy được cố tình giữ nguyên cho đến khi mỗi liên kết cứng mới được tạo. 

Việc sắp xếp thư mục sử dụng`path.count('/')`như một thước đo độ sâu. Bởi vì mọi đường dẫn thư mục đều kết thúc bằng`/`, một đứa trẻ luôn có số dấu gạch chéo lớn hơn cha mẹ của nó. Độ sâu số chính xác là không liên quan, chỉ có vấn đề thứ tự. 

Các nhóm hoạt động được phát ra theo một thứ tự cố định. Việc tạo thư mục được ưu tiên hàng đầu vì các liên kết và thư mục lồng nhau có thể phụ thuộc vào chúng. Liên kết đến trước khi hủy liên kết, vì có thể cần một nguồn lỗi thời. Hủy liên kết đến trước`rmdir`, vì các thư mục phải trống trước khi xóa. 

Không có vấn đề tràn số nguyên trong Python và số lượng thao tác tối đa nhiều nhất là`2(n + m)`, dưới 40000 đối với các giới hạn đã cho. Bản thân đầu ra được lưu trữ trong một danh sách để có thể in số lượng thao tác trước khi thực hiện các thao tác. 

## Ví dụ đã hoạt động 

Đối với mẫu được cung cấp, thư mục chung là`/a/`. Thư mục`/a/e/`phải được tạo ra và`/f/`phải biến mất. tập tin`/a/b.txt`là một nguồn có sẵn cho tập tin mới`/a/e/c.txt`, trong khi`/a/d.txt`đã lỗi thời. 

| Bước | Hoạt động | Thư mục mới | Tập tin mới | File cũ còn sót lại | 
| --- | --- | --- | --- | --- | 
| 1 |`mkdir /a/e/`|`/a/e/`tồn tại | không |`/a/b.txt`,`/a/d.txt`| 
| 2 |`link /a/b.txt /a/e/c.txt`|`/a/e/`tồn tại |`/a/e/c.txt`tồn tại |`/a/b.txt`,`/a/d.txt`| 
| 3 |`unlink /a/b.txt`| không thay đổi |`/a/e/c.txt`tồn tại |`/a/d.txt`| 
| 4 |`unlink /a/d.txt`| không thay đổi |`/a/e/c.txt`tồn tại | không | 
| 5 |`rmdir /f/`| thư mục cuối cùng vẫn còn | tập tin cuối cùng vẫn còn | không | 

Chuỗi kết quả có năm thao tác, mức tối thiểu giống như đầu ra mẫu. Thứ tự khác với ví dụ của câu lệnh, điều này được cho phép vì mọi chuỗi hợp lệ tối thiểu đều được chấp nhận. 

Ví dụ thứ hai, hãy xem xét việc di chuyển một tệp giữa hai cây thư mục lồng nhau.```
6 6
/old/
/old/sub/
/old/sub/file h
/new/
/new/sub/
/new/sub/file2 h
```Hai cây thư mục không có thư mục chung và chỉ có tệp được đổi tên. 

| Bước | Hoạt động | Đã tạo thư mục | Nguồn hiện có | Tập tin lỗi thời | 
| --- | --- | --- | --- | --- | 
| 1 |`mkdir /new/`|`/new/`|`/old/sub/file`| không | 
| 2 |`mkdir /new/sub/`|`/new/`,`/new/sub/`|`/old/sub/file`| không | 
| 3 |`link /old/sub/file /new/sub/file2`| cả hai thư mục mới |`/old/sub/file`| không | 
| 4 |`unlink /old/sub/file`| không thay đổi | không cần thiết | không | 
| 5 |`rmdir /old/sub/`| không thay đổi | không | cây con cũ bị loại bỏ một phần | 
| 6 |`rmdir /old/`| cây cuối cùng đạt | không | cây con cũ đã biến mất | 

Ví dụ này thể hiện cả thứ tự độ sâu.`/new/`phải đi trước`/new/sub/`, trong khi`/old/sub/`phải đi trước`/old/`trong trình tự xóa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + m) log(n + m) · L) | Phân loại bảng băm trung bình là tuyến tính và việc sắp xếp thư mục/tệp chiếm ưu thế | 
| Không gian | O(n + m) | Tất cả các đường dẫn, hàm băm và hoạt động được tạo đều được lưu trữ | 

Đây`L <= 256`là độ dài đường dẫn tối đa. Với nhiều nhất`10^4`các đối tượng trong mỗi ảnh chụp nhanh, sắp xếp tối đa`2 * 10^4`các chuỗi ngắn dễ dàng nằm trong giới hạn và số lượng thao tác được tạo ở dưới`4 * 10^4`. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    input = sys.stdin.readline

    n, m = map(int, input().split())

    a_files = {}
    a_dirs = set()

    for _ in range(n):
        parts = input().split()
        path = parts[0]
        if path.endswith('/'):
            a_dirs.add(path)
        else:
            a_files[path] = parts[1]

    b_files = {}
    b_dirs = set()

    for _ in range(m):
        parts = input().split()
        path = parts[0]
        if path.endswith('/'):
            b_dirs.add(path)
        else:
            b_files[path] = parts[1]

    source_by_hash = {}

    for path in a_files.keys() & b_files.keys():
        source_by_hash[a_files[path]] = path

    for path, h in a_files.items():
        if h not in source_by_hash:
            source_by_hash[h] = path

    add_dirs = sorted(
        b_dirs - a_dirs,
        key=lambda p: (p.count('/'), p)
    )
    remove_dirs = sorted(
        a_dirs - b_dirs,
        key=lambda p: (-p.count('/'), p)
    )
    add_files = sorted(b_files.keys() - a_files.keys())
    remove_files = sorted(a_files.keys() - b_files.keys())

    operations = []

    for path in add_dirs:
        operations.append(f"mkdir {path}")

    for target in add_files:
        operations.append(
            f"link {source_by_hash[b_files[target]]} {target}"
        )

    for path in remove_files:
        operations.append(f"unlink {path}")

    for path in remove_dirs:
        operations.append(f"rmdir {path}")

    sys.stdout.write(
        str(len(operations)) + '\n' +
        '\n'.join(operations) +
        ('\n' if operations else '')
    )

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

sample1 = """\
4 3
/a/
/a/b.txt hash1
/a/d.txt hash2
/f/
/a/
/a/e/
/a/e/c.txt hash1
"""

expected1 = """\
5
mkdir /a/e/
link /a/b.txt /a/e/c.txt
unlink /a/b.txt
unlink /a/d.txt
rmdir /f/
"""

assert run(sample1) == expected1, "provided sample"

assert run("0 0\n") == "0\n", "minimum-size empty snapshots"

sample2 = """\
4 4
/a h
/b h
/c/
c
"""

# The previous test deliberately is not valid path input, so use a valid
# all-equal-hash case instead.
sample2 = """\
2 2
/a h
/b h
/c h
/d h
"""

expected2 = """\
4
link /a /c
link /a /d
unlink /a
unlink /b
"""

assert run(sample2) == expected2, "all equal hashes"

sample3 = """\
3 3
/old/
/old/sub/
/old/sub/file h
/new/
/new/sub/
/new/sub/file2 h
"""

expected3 = """\
6
mkdir /new/
mkdir /new/sub/
link /old/sub/file /new/sub/file2
unlink /old/sub/file
rmdir /old/sub/
rmdir /old/
"""

assert run(sample3) == expected3, "nested directory ordering"

deep_name = "/" + "a/" * 126
deep_file_a = deep_name + "x"
deep_file_b = deep_name + "y"

sample4 = (
    "1 1\n"
    + deep_file_a + " h\n"
    + deep_file_b + " h\n"
)

out4 = run(sample4).splitlines()
assert out4[0] == "2", "deep path operation count"
assert out4[1] == f"link {deep_file_a} {deep_file_b}"
assert out4[2] == f"unlink {deep_file_a}"

# Maximum-size test: 10000 old files and 10000 new files,
# all having the same hash.
old_files = [f"/a{i}" for i in range(10000)]
new_files = [f"/b{i}" for i in range(10000)]

max_input = (
    "10000 10000\n"
    + ''.join(path + " h\n" for path in old_files)
    + ''.join(path + " h\n" for path in new_files)
)

max_out = run(max_input).splitlines()

assert max_out[0] == "20000", "maximum-size operation count"
assert len(max_out) == 20001, "maximum-size output length"

link_count = sum(line.startswith("link ") for line in max_out[1:])
unlink_count = sum(line.startswith("unlink ") for line in max_out[1:])

assert link_count == 10000, "maximum-size link count"
assert unlink_count == 10000, "maximum-size unlink count"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0 0`|`0`| Đầu vào có kích thước tối thiểu và trường hợp không có gì thay đổi | 
| Hai tệp cũ và hai tệp mới có hàm băm`h`|`4`hoạt động | Sử dụng lại một nguồn cho một số liên kết cứng và trì hoãn việc hủy liên kết của nó | 
| Lồng nhau`/old/sub/`ĐẾN`/new/sub/`|`6`hoạt động | Ưu tiên phụ huynh`mkdir`và ưu tiên trẻ em`rmdir`| 
| Đường dẫn tệp gần giới hạn 256 ký tự |`2`hoạt động | Ranh giới độ dài đường dẫn và xử lý đường dẫn chính xác | 
| 10000 tệp cũ và 10000 tệp mới có cùng hàm băm |`20000`hoạt động | Kích thước đầu vào tối đa, hàm băm hoàn toàn bằng nhau và khả năng mở rộng | 

## Vỏ cạnh 

Trường hợp trống không cần hoạt động đặc biệt. Vì```
0 0
```cả hai bộ thư mục và tập tin đều trống, vì vậy cả bốn bộ khác nhau đều trống. Thuật toán không phát ra thao tác và in`0`. 

Khi tệp nguồn nằm trong thư mục sẽ bị xóa, tệp đó phải được liên kết trước khi thư mục cũ của nó bị xóa. Vì```
3 3
/old/
/old/sub/
/old/sub/file h
/new/
/new/sub/
/new/sub/file2 h
```thuật toán đầu tiên tạo ra`/new/`Và`/new/sub/`, sau đó tạo ra`/new/sub/file2`từ`/old/sub/file`, và chỉ sau đó mới xóa tệp cũ. Thứ tự thư mục có chiều sâu ngược sẽ loại bỏ`/old/sub/`trước`/old/`. Sáu thao tác chính xác là bốn thay đổi thư mục không thể tránh khỏi cộng với một`link`và một`unlink`. 

Một số tệp mới có cùng hàm băm không yêu cầu một số nguồn gốc độc lập. Vì```
2 2
/a h
/b h
/c h
/d h
```bản đồ nguồn chọn`/a`cho hàm băm`h`. Thuật toán tạo ra cả hai`/c`Và`/d`dưới dạng liên kết cứng đến`/a`, sau đó loại bỏ`/a`Và`/b`. Đầu ra là bốn hoạt động. Sự thật là`/a`cuối cùng có bị xóa không thành vấn đề vì tất cả các liên kết sử dụng nó đều đã được tạo rồi. 

Một tệp không thay đổi phải được loại trừ khỏi cả hai tập hợp khác biệt của tệp. Vì```
1 1
/a h
/a h
```

`/a`xảy ra trong cả hai từ điển có cùng hàm băm, vì vậy cả hai đều không`link`cũng không`unlink`được tạo ra. Đầu ra là`0`. Đây là lý do tại sao chỉ so sánh các giá trị băm sẽ không chính xác: chính đường dẫn sẽ xác định xem tên tệp đã tồn tại ở trạng thái mong muốn hay chưa. 

Các đường dẫn hợp lệ sâu nhất được xử lý mà không cần đệ quy. Việc thực hiện sử dụng số lượng`/`chỉ để sắp xếp các hoạt động thư mục, do đó, ngay cả một đường dẫn gần với giới hạn 256 ký tự cũng được xử lý như một chuỗi thông thường. Cha mẹ có ít dấu gạch chéo hơn con cháu của nó, đủ để thiết lập thứ tự tạo và xóa cần thiết.
