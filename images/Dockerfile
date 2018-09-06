FROM registry.fedoraproject.org/fedora:28

WORKDIR /


COPY config/ /config/

RUN dnf install -y mtools qemu-img dosfstools && \
    qemu-img create -f raw /disk.raw 2M && \
    mkfs.vfat -n config-2 -I /disk.raw && \
    mmd -i /disk.raw ::openstack && \
    mmd -i /disk.raw ::openstack/latest && \
    mcopy -i /disk.raw /config/vendor_data.json ::openstack/latest/vendor_data.json && \
    mcopy -i /disk.raw /config/meta_data.json ::openstack/latest/meta_data.json && \
    mcopy -i /disk.raw /config/user_data ::openstack/latest/user_data


FROM kubevirt/registry-disk-v1alpha:latest

COPY --from=0 /disk.raw /disk

