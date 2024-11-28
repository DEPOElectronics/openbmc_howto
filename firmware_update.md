busctl set-property xyz.openbmc_project.Software.BMC.Updater \
    /xyz/openbmc_project/software/<id> \
    xyz.openbmc_project.Software.Activation RequestedActivation s \
    xyz.openbmc_project.Software.Activation.RequestedActivations.Active